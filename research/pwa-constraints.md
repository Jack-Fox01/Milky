# iOS Safari / Android Chrome constraints for a local-first PWA

Research for Jack-Fox01/Milky#3. Facts only; decisions belong to other tickets.

**Snapshot date: 2026-10-22.**

Versions this is true for:

| Platform | Current version at snapshot | Notes |
| --- | --- | --- |
| iOS / iPadOS | **27** (iOS 26.x still widely deployed) | Apple's iPhone User Guide version selector offers iOS 27 and iOS 26 ([Apple](https://support.apple.com/guide/iphone/open-as-web-app-iphea86e5236/ios)) |
| Safari (iOS/macOS) | **27.0** shipped Sept 2026; **26.6** is the last 26.x release | [WebKit blog](https://webkit.org/blog/18325/webkit-features-for-safari-27-0/), [26.6](https://webkit.org/blog/18178/webkit-features-for-safari-26-6/) |
| Android Chrome | **156** stable since 2026-10-20 | [Chrome Platform Status 156](https://chromestatus.com/release-notes/156); [Chrome Releases, Oct 2026](https://chromereleases.googleblog.com/2026/10/) |
| Compat data | fetched from `mdn/browser-compat-data@main` on 2026-10-22 | versions below are from that snapshot |

Safari↔iOS mapping used throughout: Safari 16.4 = iOS 16.4, Safari 18.4 = iOS 18.4, Safari 26.0 = iOS 26.0, Safari 27.0 = iOS 27.

---

## 1. Install (Add to Home Screen) and what installed gets you

### iOS

- Install is always the user's Share-sheet action **Add to Home Screen**; there is no programmatic install prompt. `beforeinstallprompt` does not exist in Safari (`safari: false` in [BCD `Window.beforeinstallprompt_event`](https://github.com/mdn/browser-compat-data/blob/main/api/Window.json)); it is Chromium-only ([web.dev install criteria](https://web.dev/articles/install-criteria)).
- Since **iOS 26**, *every* site added to the Home Screen opens as a web app by default; the user can turn "Open as Web App" off. WebKit's own words: "there are now zero requirements for 'installability' in Safari" ([WebKit, Safari 26.0](https://webkit.org/blog/17333/webkit-features-in-safari-26-0/)).
- Before iOS 26 the site needed a manifest with `display: standalone|fullscreen`, or the legacy `apple-mobile-web-app-capable` meta tag; otherwise the icon opened as a bookmark in the browser ([WebKit 26.0 blog](https://webkit.org/blog/17333/webkit-features-in-safari-26-0/), [WebKit web push post](https://webkit.org/blog/13878/web-push-for-web-apps-on-ios-and-ipados/)).
- Icons: manifest `icons` supported since iOS 15.4; `apple-touch-icon` still takes precedence when both are present ([WebKit](https://webkit.org/blog/13878/web-push-for-web-apps-on-ios-and-ipados/)).

**What an installed web app gets that a Safari tab does not:**

1. Standalone window, own entry in the App Switcher, separate from Safari ([WebKit](https://webkit.org/blog/13878/web-push-for-web-apps-on-ios-and-ipados/)).
2. **Separate storage container** — installed-app data is not shared with Safari, and vice versa ([WebKit bug 181849](https://bugs.webkit.org/show_bug.cgi?id=181849); [Tracking Prevention](https://webkit.org/tracking-prevention/) says home-screen web app data "is kept isolated from Safari").
3. **Exemption from ITP's 7-day storage deletion** (see §3).
4. **Notifications, Web Push, and icon badging** (iOS 16.4+), which do not exist in a Safari tab at all (see §6).
5. Same storage quota class as the browser app (60% of disk), not the 15% WKWebView class ([WebKit storage policy](https://webkit.org/blog/14403/updates-to-storage-policy/)).
6. Screen Wake Lock actually works in standalone apps from **iOS 18.4** (see §7).

### Android Chrome

- Install criteria ([web.dev, updated 2024-09-19](https://web.dev/articles/install-criteria)): HTTPS, not already installed, engagement heuristics (a click/tap at some point, ≥30 s on the page), and a manifest with `name`/`short_name`, `icons` incl. 192 px + 512 px, `start_url`, `display` in `fullscreen|standalone|minimal-ui|window-controls-overlay`, and no `prefer_related_applications: true`. The service-worker requirement that older docs listed is **not** in the current criteria page.
- `beforeinstallprompt` fires when those criteria are met, letting the app show its own install UI ([web.dev](https://web.dev/articles/install-criteria)).
- Installing creates a **WebAPK**: launcher entry, Android app-settings entry, intent filters for in-scope URLs ([web.dev WebAPKs](https://web.dev/articles/webapks)).
- Chrome **does not segregate** installed-app storage from the browser profile: "Chrome uses the current profile to store any data, and it will not be segregated away… Cookies are shared and active, any client side storage is accessible" ([web.dev WebAPKs](https://web.dev/articles/webapks)). Same-origin tab and app are the same storage bucket.

---

## 2. Offline app shell, service workers, updates

- Service workers have shipped in Safari since iOS 11.3; an offline shell is a service worker + Cache Storage / IndexedDB (MDN/service worker docs). Nothing about iOS requires a service worker to install (see §1), but it is what makes offline work.
- **Update check triggers**: a navigation to an in-scope page; a functional event (`push`, `sync`) if no check happened in the last 24 h; or a manual `registration.update()`. Byte-different script = new worker ([web.dev: service worker lifecycle](https://web.dev/articles/service-worker-lifecycle)).
- **A new worker "waits"** until the old one controls zero clients. During that window the old cache keeps serving — that is how staleness becomes visible: the user opens the app, gets the old shell, and nothing tells them ([web.dev](https://web.dev/articles/service-worker-lifecycle)).
  - `self.skipWaiting()` + `clients.claim()` (or `skipWaiting()` on a user action) shortens this; the trade-off is that two versions can be live at once and must share storage safely ([web.dev](https://web.dev/articles/service-worker-lifecycle)).
  - There is no built-in "update available" UI. Milky must render it itself.
- Practical iOS shape: launching the home-screen app is a navigation, so an update check happens at launch; a waiting worker still needs every client (the app window) to go away before it activates, which on iOS effectively means the user closes the app and reopens it, or the app calls `skipWaiting()`.
- Safari 27.0 added the **Service Worker static routing API** (bypass the worker for some requests) — no behavioural change for updates ([WebKit 27.0](https://webkit.org/blog/18325/webkit-features-for-safari-27-0/)).
- **Background Sync and Periodic Background Sync do not exist in Safari** (`SyncManager`, `PeriodicSyncManager`: `safari: false`, `safari_ios: mirror` in BCD). There is no way to refresh the shell or data while the app is closed on iOS.

---

## 3. Client-side storage

### Quotas (current)

| | Safari / installed iOS web app (Safari 17+) | Chrome Android |
| --- | --- | --- |
| Per-origin quota | ~60% of total disk (browser-app class; installed web apps use the same class) | ~60% of total disk |
| Overall browser quota | ~80% of disk (browser apps) | ~80% of disk |
| localStorage | 5 MiB per origin | 5 MiB per origin |
| Source | [WebKit storage policy](https://webkit.org/blog/14403/updates-to-storage-policy/), [MDN](https://developer.mozilla.org/en-US/docs/Web/API/Storage_API/Storage_quotas_and_eviction_criteria) | [MDN](https://developer.mozilla.org/en-US/docs/Web/API/Storage_API/Storage_quotas_and_eviction_criteria) |

- Over quota: `QuotaExceededError`; must be caught ([MDN](https://developer.mozilla.org/en-US/docs/Web/API/Storage_API/Storage_quotas_and_eviction_criteria)).
- `navigator.storage.estimate()`: Safari 17.0+ (WebKit calls the Storage API "fully supported" from 17.0).
- **Conflict to flag:** web.dev's [Storage for the web](https://web.dev/articles/storage-for-the-web) still says Safari allows "about 1GB" and prompts in 200 MB increments — it predates Safari 17. WebKit's 2023 policy post is the authority: Safari 17 no longer prompts, quota is disk-relative. Use WebKit.

### localStorage vs IndexedDB

- `localStorage` is synchronous, string-only, ~5 MiB, blocks the main thread, and is not accessible from workers/service workers ([web.dev](https://web.dev/articles/storage-for-the-web), [MDN](https://developer.mozilla.org/en-US/docs/Web/API/Storage_API/Storage_quotas_and_eviction_criteria)).
- IndexedDB is asynchronous, worker-accessible, and the intended store for app data ([web.dev](https://web.dev/articles/storage-for-the-web)).
- Both fall under the same eviction rule on iOS (see next).

### Eviction

- **Safari, deliberate expiry:** "ITP deletes all cookies created in JavaScript and all other script-writeable storage after 7 days of no user interaction with the website", where the storage forms are IndexedDB, localStorage, sessionStorage, media keys, **and service worker registrations and cache** ([Tracking Prevention in WebKit](https://webkit.org/tracking-prevention/)); introduced in iOS/iPadOS 13.4 / Safari 13.1 ([web.dev](https://web.dev/articles/storage-for-the-web)).
  - The clock is **7 days of browser use** without a user interaction (click/tap/keyboard); scrolling does not count ([Tracking Prevention](https://webkit.org/tracking-prevention/)).
  - **Exemption: the first-party domain of a home-screen web application is skipped entirely** by that deletion, and its data is isolated from Safari ([Tracking Prevention](https://webkit.org/tracking-prevention/)). So: Safari tab → wiped after ~7 idle days; installed web app → not wiped by this rule.
- **Storage-pressure eviction (both platforms):** LRU per origin, whole-origin deletion, best-effort storage only; `persist()`ed origins are skipped ([MDN](https://developer.mozilla.org/en-US/docs/Web/API/Storage_API/Storage_quotas_and_eviction_criteria), [WebKit](https://webkit.org/blog/14403/updates-to-storage-policy/)). This applies to installed web apps too — installation protects you from the 7-day rule, not from disk pressure.
- Chrome: LRU eviction when the browser exceeds its budget or the device is low on space ([web.dev](https://web.dev/articles/storage-for-the-web)).

### Persistent storage on iOS

- `navigator.storage.persist()` / `persisted()` exist in Safari from **15.2** and are fully supported from **17.0** (BCD; [WebKit](https://webkit.org/blog/14403/updates-to-storage-policy/)).
- WebKit **grants by heuristic, with no prompt**; the stated heuristic includes "whether the website is opened as a Home Screen Web App" ([WebKit storage policy](https://webkit.org/blog/14403/updates-to-storage-policy/)). MDN: Safari and Chromium "automatically approve or deny the request based on the user's history of interaction with the site and do not show any prompts".
- Consequence: on iOS, `persist()` returning `true` is likely **only** in the installed app; in a Safari tab it may silently return `false`. Chrome grants when the site is installed/bookmarked/notifications-enabled or has high engagement ([web.dev](https://web.dev/articles/persistent-storage)).
- There is no way to revoke persistence programmatically ([web.dev](https://web.dev/articles/persistent-storage)).

### After 7 days of non-use

| | Data in Safari tab | Data in installed web app |
| --- | --- | --- |
| localStorage | deleted | kept (ITP skips the domain) |
| IndexedDB | deleted | kept |
| Cache Storage + SW registration | deleted | kept |
| JS-set cookies | deleted | kept |

(Source: [Tracking Prevention in WebKit](https://webkit.org/tracking-prevention/).) This is the single strongest argument for asking the caregiver to install.

---

## 4. Audio

### Autoplay / user gesture

- iOS has required a user gesture for audible `<audio>`/`<video>` playback since the beginning; iOS 10 relaxed it only for *muted* or *audio-track-free* video ([WebKit, New video policies for iOS](https://webkit.org/blog/6784/new-video-policies-for-ios/)). WebKit's definition of a gesture: the JS that calls `play()` must run inside a handler for `touchend`, `click`, `dblclick` or `keydown`. `canplaythrough` → `play()` is not a gesture.
- Web Audio: starting a source node outside a user-input handler is subject to the same autoplay rules ([MDN autoplay guide](https://developer.mozilla.org/en-US/docs/Web/Media/Guides/Autoplay)).
- Chrome Android: audible autoplay requires prior user interaction with the domain (or MEI allowlisting); muted autoplay is always allowed ([MDN autoplay guide](https://developer.mozilla.org/en-US/docs/Web/Media/Guides/Autoplay)).
- **Nothing in the specs promises a permanent "unlock".** `play()` returns a promise that rejects with `NotAllowedError` when blocked; the reliable pattern is to fire the sound directly from the tap that logs the feed ([MDN](https://developer.mozilla.org/en-US/docs/Web/Media/Guides/Autoplay)). [INFERENCE] A gesture that already happened usually allows later programmatic playback in the same document, but no primary source guarantees it for the session, so don't depend on it.

### Silent switch (iOS)

Authoritative statement from a WebKit engineer ([bug 252746](https://bugs.webkit.org/show_bug.cgi?id=252746)):

- `<audio>` / `<video>` playback behaves as the system "playback" category: **ignores the ring/silent switch**, interrupts other audio.
- Web Audio is treated as "ambient": **obeys the mute switch**, mixes with other audio, and **stops when the app is backgrounded**.
- The **Audio Session API** exists to fix this: `navigator.audioSession.type = "playback"` makes Web Audio behave like media. `AudioSession` is in Safari **16.4+** (BCD: `safari 16.4`, `safari_ios mirror`); MDN labels the API experimental ([MDN](https://developer.mozilla.org/en-US/docs/Web/API/Audio_Session_API), [type values](https://developer.mozilla.org/en-US/docs/Web/API/AudioSession/type)).

So: a Web Audio chime is silent when the phone's mute switch is on unless the session type is set; an `<audio>` element is not.

---

## 5. Haptics / vibration

- `navigator.vibrate()`: Chrome Android **32+**; **Safari and iOS Safari: not supported** (BCD `api.Navigator.vibrate`: `safari: false`, `safari_ios: mirror`).
- There is no alternative haptics API for web content on iOS (no `navigator.haptic*`; not in BCD, not in WebKit's feature lists through Safari 27.0).
- Practical answer: **no haptics on iOS at all**; Android gets vibration only.

---

## 6. Notifications — the decisive section

### What is available where

| Capability | Safari tab (iOS) | Installed web app (iOS) | Chrome Android (tab or installed) |
| --- | --- | --- | --- |
| `Notification` interface exists | **No** — "The parent `Notification` interface is undefined unless the page is a web app saved to the home screen. The app's manifest must have a non-default `display` value" (BCD note, `safari_ios 16.4`) | Yes, iOS **16.4+** | Yes (Chrome 42+) |
| `new Notification()` constructor | n/a | Throws `ReferenceError` unless installed (same BCD note); generally use the SW path | **Always throws `TypeError`** — must use `ServiceWorkerRegistration.showNotification()` (BCD note) |
| Web Push (`PushManager.subscribe`) | No | Yes, iOS **16.4+** (BCD `safari_ios 16.4`; [WebKit](https://webkit.org/blog/13878/web-push-for-web-apps-on-ios-and-ipados/)) | Yes |
| Icon badge (`setAppBadge`) | No | Yes, iOS **16.4+** ([WebKit](https://webkit.org/blog/13878/web-push-for-web-apps-on-ios-and-ipados/)) | Yes |
| Delivery when app is closed | — | Yes, via APNs ([WebKit](https://webkit.org/blog/13878/web-push-for-web-apps-on-ios-and-ipados/)) | Yes — Push API delivers "whether or not the web app is in the foreground, or even currently loaded" ([MDN](https://developer.mozilla.org/en-US/docs/Web/API/Push_API)) |
| **Schedule a notification for later, no server** | No | **No** | **No** (see below) |

### Permission flow

- iOS: permission can only be requested from an installed web app, and only "in response to direct user interaction — such as tapping on a 'subscribe' button" ([WebKit](https://webkit.org/blog/13878/web-push-for-web-apps-on-ios-and-ipados/)). The OS then shows its normal notification prompt; afterwards the user manages it per web app in Settings, like any app.
- WebKit requires `userVisibleOnly: true` on subscription and **revokes the subscription** if a push arrives and no notification is shown ([Declarative Web Push](https://webkit.org/blog/16535/meet-declarative-web-push/)). Silent push is not a thing on iOS.
- Android: `Notification.requestPermission()` → Chrome prompt; Android 13+ additionally has the OS-level `POST_NOTIFICATIONS` runtime permission (API 33), so the user may have to grant twice or the OS prompt appears on top ([Android docs](https://developer.android.com/about/versions/13/behavior-changes-all)). Chrome may show a quieter prompt UI for sites with low engagement; the practical pattern is still a request from a user action.

### Web push vs local notifications

- "Local notification" in the native sense (an OS-scheduled alarm) **does not exist on the web platform**.
  - The only scheduling proposal, **Notification Triggers** (`showTrigger` / `TimestampTrigger`), never shipped. Chrome's own docs: status "Launch: Not started", usable only behind `#enable-experimental-web-platform-features`, last updated 2019 ([Chrome for Developers](https://developer.chrome.com/docs/web-platform/notification-triggers)); Chrome Platform Status still shows "In developer trial (Behind a flag)", not shipped, entry last updated 2022 ([chromestatus](https://chromestatus.com/feature/5133150283890688), API payload). The docs' source markdown carries the note "The development of Notification Triggers API… is no longer pursued" ([GitHub](https://github.com/GoogleChrome/developer.chrome.com/blob/main/site/en/docs/web-platform/notification-triggers/index.md)). **Conflict:** the timer-throttling article still recommends it as a workaround; that page is stale (2021) — believe the platform-status data.
  - Safari never implemented it (`showTrigger` absent from BCD).
- Therefore a reminder can only be produced by **something running at that moment**: the page (open), or a service worker woken by a push message.
- iOS: push messages go through **APNs**, sent by *your server* to the subscription endpoint; no Apple Developer Program membership required, but a server (or any HTTPS sender holding the VAPID private key) is mandatory ([WebKit](https://webkit.org/blog/13878/web-push-for-web-apps-on-ios-and-ipados/)).
- **Declarative Web Push** (iOS/iPadOS **18.4+**, [WebKit](https://webkit.org/blog/16535/meet-declarative-web-push/)) removes the *service worker* requirement — `window.pushManager`, a JSON body with `web_push: 8030`, browser-rendered notification — but **not** the server: it is still an HTTP POST to a push endpoint.

**Bottom line for a local-only app with no server:** it **cannot** schedule or deliver any notification while it is closed, on any current browser. It can show a notification immediately, while open, from the service worker (`registration.showNotification()`), and on iOS that still requires the app to be installed and permission granted.

---

## 7. Background timers and wake lock

- **iOS suspends the app.** Background apps "must do as little work as possible, and preferably nothing"; UIKit "moves the associated scene to the background state and eventually to the suspended state" and may disconnect it at any time ([Apple: Managing your app's life cycle](https://developer.apple.com/documentation/uikit/managing-your-app-s-life-cycle)). A home-screen web app is such an app (WebKit describes it as a ViewService, not a `UIApplication` — [bug 254545 comment](https://bugs.webkit.org/show_bug.cgi?id=254545)). Suspended = no code runs: no `setTimeout`, no intervals, no countdown.
  - Consequence: any timer-based reminder is dead while the app is backgrounded; recompute state from timestamps on resume/launch (`visibilitychange`, `pageshow`).
- There is no background sync on iOS (§2), so no periodic wake-up either.
- **Chrome Android** throttles hidden tabs: chained timers → 1/s when hidden, and **1/min** once hidden >5 min, chain ≥5, and silent ≥30 s ([Chrome 88](https://developer.chrome.com/blog/timer-throttling-in-chrome-88)). Push still wakes the service worker.
- **Screen Wake Lock** (`navigator.wakeLock.request()`):
  - Chrome Android **84+**.
  - Safari **16.4+** in a tab, but **broken in standalone home-screen apps from 16.4 through 18.3** — "Fixed Wake Lock API for Home Screen Web Apps" in [Safari 18.4 release notes](https://developer.apple.com/documentation/safari-release-notes/safari-18_4-release-notes); long-running [WebKit bug 254545](https://bugs.webkit.org/show_bug.cgi?id=254545) documents the gap and the fix landing in 18.4. BCD models this as `safari_ios 16.4–18.3` partial (does not work in standalone), `18.4+` full.
  - Wake lock does not survive backgrounding; it must be re-acquired on `visibilitychange` (BCD/MDN behaviour; the same bug thread notes a separate re-acquire issue).

---

## 8. Data across updates, reinstall, uninstall

- **App updates do not touch storage.** A service worker update replaces script and caches only; IndexedDB/localStorage are untouched unless the app's own code migrates them (that is what the `activate` event is for — [web.dev](https://web.dev/articles/service-worker-lifecycle)).
- **iOS, installed app:** data lives in the web app's own container, isolated from Safari ([WebKit bug 181849](https://bugs.webkit.org/show_bug.cgi?id=181849), [Tracking Prevention](https://webkit.org/tracking-prevention/)). Removing the icon deletes the app; MDN notes "The deletion confirmation provides information as to whether deleting the icon deletes a bookmark or entire application" ([MDN](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Installing)).
  - **Gap in the public docs:** Apple/WebKit publish no statement about whether removing the icon also deletes the web app's stored data. Community reports conflict (some users find cookies survive a remove/re-add; others lose IndexedDB). Treat uninstall as destructive and **design an export/backup path**; do not rely on data surviving.
  - Also note the isolation trap: data entered in a Safari tab is **not** visible to the installed app. Installing after using the tab side looks like data loss.
- **Android Chrome:** installed app shares the browser profile's storage ([web.dev](https://web.dev/articles/webapks)). Uninstalling the WebAPK removes the app shell; the site data lives in Chrome's profile, and the Chrome-side question ("Will my installed site's storage be cleared if the user clears Chrome's cache?" → **"Yes"**) is answered in the same doc. There is no documented promise that uninstalling the app clears site data — do not assume either direction; `Start > Clear browsing data` definitely does.

---

## 9. What this means for Milky

| Requirement | Verdict | Fallback |
| --- | --- | --- |
| Reminder / "overdue feed" notification while the app is closed, with no server | **Impossible** on iOS and Android. No scheduling API exists (Notification Triggers abandoned), and push requires a server POST. | Compute overdue state from timestamps on every launch/resume and show an in-app banner first thing. Optionally: badge the icon (`setAppBadge`, iOS 16.4+ installed, requires notification permission to be *displayed*); offer a downloadable `.ics` reminder for the phone's Calendar (user-created, outside the web platform). Adding a tiny push sender is the only real fix and contradicts "no server". |
| Notification without installing on iOS | **Impossible.** `Notification` is undefined in a Safari tab. | Ask the user to Add to Home Screen first; or drop notifications entirely. |
| Notification that fires exactly on time | Impossible without push; even push is best-effort (APNs can delay). | In-app only. |
| Guaranteed data persistence | Not guaranteed. `persist()` is heuristic (no prompt, no override), and storage-pressure eviction applies even to installed apps. | Call `persist()` after the first feed is saved, call `persisted()` and show status; require install (7-day rule otherwise); ship JSON export/import. |
| Data survives 7 days of non-use | Fails **in a Safari tab** (localStorage, IndexedDB, SW, cache all deleted). | Install to Home Screen (domain exempt from the 7-day rule). |
| Data survives uninstall | Undocumented on iOS; no promise on Android. | Export/backup; don't market reinstall as safe. |
| Celebration sound plays even with the ring/silent switch on | `<audio>`/`<video>`: yes. Web Audio: **no** (ambient category obeys the mute switch). | Play the celebration through an `<audio>` element, or set `navigator.audioSession.type = "playback"` (Safari 16.4+) before Web Audio. |
| Celebration sound with no user gesture | **Impossible** on iOS (and audible autoplay is blocked in Chrome too). | The celebration follows the tap that logs the feed — fire it inside that handler; keep a "play" affordance if it fails. |
| Haptic feedback on iOS | **Impossible.** `navigator.vibrate` is unsupported. | Visual/audio feedback; vibration on Android only. |
| Programmatic install prompt on iOS | **Impossible.** No `beforeinstallprompt`. | Show "Share → Add to Home Screen" instructions. |
| Screen stays awake during a feed entry | Works in a Safari tab on iOS 16.4+, in an installed app only on **iOS 18.4+**; Chrome 84+. | Set minimum iOS 18.4 if wake lock is used in the installed app; otherwise advance the UI quickly / don't depend on it. |
| Background countdown / interval ticks | iOS suspends the app; no background sync. | Derive everything from `Date.now()` deltas on resume. |

### Practical minimum OS versions

- **iOS 16.4** — the floor for anything notification-related (push, permission, badge) and for Wake Lock in a Safari tab.
- **iOS 17.0** — Storage API (`estimate`, `persist`) fully supported, disk-relative quotas.
- **iOS 18.4** — Wake Lock works in home-screen web apps; Declarative Web Push available.
- Recommendation to feed the spec tickets: target **iOS 18.4+** if wake lock/standalone behaviour matters; otherwise 16.4 is the hard floor for reminders. **Chrome Android 84+** (wake lock) / 55+ (`persist`); in practice anything current (Chrome 156 at snapshot).

---

## 10. Conflicts between authorities, and where guidance is silent

1. **Safari quota** — web.dev "Storage for the web" says ~1 GB + user prompts; WebKit's 2023 storage-policy post says 60% of disk from Safari 17 with no prompts. WebKit wins; web.dev is stale.
2. **Notification Triggers** — Chrome docs page describes it as usable; chromestatus shows "developer trial (behind a flag)", not shipped, last touched 2022, and the docs' markdown source says development "is no longer pursued". Treat as nonexistent.
3. **Wake Lock on iOS** — the Safari 16.4 announcement lists Screen Wake Lock as a feature; in practice it never worked in home-screen web apps until 18.4 (WebKit bug 254545; Safari 18.4 release notes). Both statements are "true"; the installed-app case is the one that matters.
4. **Removing an iOS home-screen web app** — no Apple/WebKit statement on whether stored data is deleted. MDN hedges ("the deletion confirmation provides information as to whether…"). Unverified.
5. **Silent background push on iOS** — impossible by design (`userVisibleOnly: true`, subscription revoked otherwise). No "wake up and compute" path, which rules out a server-side scheduler that stays quiet.
6. **iOS 26 "Open as Web App" default** — starts 2026, any site can be installed without a manifest. Whether the `Notification` interface still requires a manifest with non-default `display` (BCD's 16.4 note) in iOS 26/27 is **not documented**; ship a manifest with `display: standalone` to be safe.
7. **Chrome install criteria and service workers** — older docs required a service worker with a fetch handler; the current criteria page (2024-09-19) does not. Offline still needs one, but installability does not.

---

## Sources

- WebKit — [Web Push for Web Apps on iOS and iPadOS (2023-02-16)](https://webkit.org/blog/13878/web-push-for-web-apps-on-ios-and-ipados/)
- WebKit — [Meet Declarative Web Push (2025-03-27)](https://webkit.org/blog/16535/meet-declarative-web-push/)
- WebKit — [Updates to Storage Policy (2023-08-10)](https://webkit.org/blog/14403/updates-to-storage-policy/)
- WebKit — [Tracking Prevention in WebKit](https://webkit.org/tracking-prevention/)
- WebKit — [WebKit Features in Safari 26.0 (2025-09-15)](https://webkit.org/blog/17333/webkit-features-in-safari-26-0/)
- WebKit — [WebKit Features for Safari 27.0](https://webkit.org/blog/18325/webkit-features-for-safari-27-0/)
- WebKit — [New `<video>` Policies for iOS (2016)](https://webkit.org/blog/6784/new-video-policies-for-ios/)
- WebKit bugs — [254545 Wake Lock in Home Screen web apps](https://bugs.webkit.org/show_bug.cgi?id=254545), [252746 muted playback](https://bugs.webkit.org/show_bug.cgi?id=252746), [181849 storage isolation](https://bugs.webkit.org/show_bug.cgi?id=181849)
- Apple — [Safari 18.4 release notes](https://developer.apple.com/documentation/safari-release-notes/safari-18_4-release-notes)
- Apple — [Managing your app's life cycle](https://developer.apple.com/documentation/uikit/managing-your-app-s-life-cycle)
- Apple — [Turn a website into an app in Safari on iPhone (iOS 27)](https://support.apple.com/guide/iphone/open-as-web-app-iphea86e5236/ios)
- MDN — [Storage quotas and eviction criteria](https://developer.mozilla.org/en-US/docs/Web/API/Storage_API/Storage_quotas_and_eviction_criteria), [Autoplay guide](https://developer.mozilla.org/en-US/docs/Web/Media/Guides/Autoplay), [Push API](https://developer.mozilla.org/en-US/docs/Web/API/Push_API), [Audio Session API](https://developer.mozilla.org/en-US/docs/Web/API/Audio_Session_API), [Installing and uninstalling web apps](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Installing)
- MDN browser-compat-data (snapshot 2026-10-22) — `api/{Notification,PushManager,WakeLock,StorageManager,Navigator,AudioSession,Window,SyncManager,PeriodicSyncManager}.json`
- Chrome for Developers — [Install criteria](https://web.dev/articles/install-criteria), [Persistent storage](https://web.dev/articles/persistent-storage), [Storage for the web](https://web.dev/articles/storage-for-the-web), [Service worker lifecycle](https://web.dev/articles/service-worker-lifecycle), [WebAPKs](https://web.dev/articles/webapks), [Timer throttling in Chrome 88](https://developer.chrome.com/blog/timer-throttling-in-chrome-88), [Notification Triggers](https://developer.chrome.com/docs/web-platform/notification-triggers), [Chrome 156](https://chromestatus.com/release-notes/156)
- Chrome Platform Status — [Notification Triggers feature 5133150283890688](https://chromestatus.com/feature/5133150283890688)
- Android — [Behavior changes: Android 13 notification permission](https://developer.android.com/about/versions/13/behavior-changes-all)
