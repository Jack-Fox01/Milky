# Daily feeding-volume guidance by age (term infants, 0–12 months)

Research for issue #2. Facts only; no product decision.

All sources fetched and quoted on **2026-10-22**. Each number carries the source URL and the
source's own "last updated" date. `[INFERENCE]` marks a conclusion I derived (e.g. arithmetic
on a source's own numbers) rather than a statement the source makes.

Unit conventions: sources publish natively in **US fluid ounces** (AAP, CDC) or **millilitres**
(NHS, WHO, BNFc). 1 US fl oz = 29.57 ml. AAP rounds 1 oz → 30 ml, 2 oz → 60 ml, 3–4 oz → 90–120 ml,
6–8 oz → 180–240 ml, and writes 32 oz as 960 ml (exactly 946 ml) — treat AAP's ml as rounded.

---

## 1. The headline answer

There is **no single authoritative ml/day figure by age band**. Authorities publish three
different shapes of rule, and they do not agree with each other:

| Rule shape | Who publishes it | Value |
|---|---|---|
| Weight-based daily total | AAP, BNFc (UK), WHO | 130–200 ml/kg/day |
| Per-feed volume × feeds per day | AAP, CDC | 30–60 ml/feed at birth → 180–240 ml/feed at 6 months |
| No number at all — responsive feeding only | NHS (main site), Unicef UK | "Your baby will know how much milk they need" |

The three weight-based figures, side by side:

| Source | Figure | Date | Scope |
|---|---|---|---|
| WHO, *Infant and young child feeding: model chapter*, 2nd ed., p. 107 | **130 ml/kg body weight per day** ("The full amount of milk normally required by a healthy term baby") | 2nd ed., © WHO 2025 | term baby |
| BNF for Children (NICE), *Enteral nutrition* | **150–200 ml/kg/day** ("Term infants with normal gastro-intestinal function are given either breast milk or normal infant formula during the first year of life. The average intake is between 150 mL and 200 mL/kg/day") | BNFC online, accessed 2026-10-22 | 0–12 months |
| AAP, HealthyChildren | **2½ fl oz per pound per day** = 74 ml per 453 g = **≈163 ml/kg/day**, with a ceiling of **32 oz (960 ml)/24 h** | Last Updated 2022-05-16 | formula |

WHO model chapter: https://iris.who.int/server/api/core/bitstreams/2966cbcb-d5a0-4ee5-b39f-4dc1bde543ba/content
BNFc: https://bnfc.nice.org.uk/treatment-summaries/enteral-nutrition/
AAP: https://www.healthychildren.org/English/ages-stages/baby/formula-feeding/Pages/amount-and-schedule-of-formula-feedings.aspx

**Conflict, not rounding.** 130 vs 150–200 ml/kg/day is a ~15–50 % spread. WHO's figure sits in a
relactation section (a supplement calculation) and is a population average for breast milk; BNFc's
is a clinical feeding range for breast milk *or* formula; AAP's is formula with a hard ceiling.
WHO's number is the outlier and should not be read as an underfeeding threshold.

---

## 2. Age-band table — formula

Everything below is *stated by the source* unless marked `[INFERENCE]` (in which case it is the
source's own per-feed figure multiplied by the source's own feeds-per-day figure).

| Age band | Feeds / 24 h | Per feed | Daily total | Source |
|---|---|---|---|---|
| Day 1–2 (bottle) | 8–12 | ½ oz (15 ml) | ~120–180 ml `[INFERENCE]` | AAP 2024-04-02 |
| Day 1–2 (formula-only) | 8–12 | 1–2 oz (30–60 ml) every 2–3 h | 240–720 ml `[INFERENCE]` | CDC 2026-04-21 |
| Day 3 | — | — | **300–400 ml** | WHO model chapter 2nd ed., p. 32 |
| Day 5 | — | — | **500–800 ml** | WHO model chapter 2nd ed., p. 32 |
| Week 1 | — | no more than 1–2 oz (30–60 ml) | — | AAP 2022-05-16 |
| 1 month | every 3–4 h (≈6–8) | 3–4 oz (90–120 ml) | **32 oz (960 ml)** — stated explicitly | AAP 2022-05-16 |
| 1–3 months | every 3–4 h (≈6–8) | ≥3–4 oz (≥120 ml) | 540–960 ml `[INFERENCE]` | AAP 2022-05-16 |
| 3–6 months | every 3–4 h | not stated per feed | 150–200 ml/kg/day; ≤960 ml/24 h | BNFc; AAP |
| 6 months | 4 or 5 | 6–8 oz (180–240 ml) | 720–1200 ml `[INFERENCE]`, **but AAP also caps at 960 ml** | AAP 2022-05-16 |
| 6–12 months | 5–6 (formula **or** solids combined) | — | "will gradually need less infant formula" | CDC 2026-04-21 |
| 12–24 months | — | — | formula stops; whole cow's milk **16 to max 24 oz (470–710 ml)/day** | CDC 2026-04-21; AAP 2022-06-02 |

AAP: https://www.healthychildren.org/English/ages-stages/baby/formula-feeding/Pages/amount-and-schedule-of-formula-feedings.aspx
AAP "How Often and How Much": https://www.healthychildren.org/English/ages-stages/baby/feeding-nutrition/Pages/how-often-and-how-much-should-your-baby-eat.aspx
CDC: https://cdc.gov/infant-toddler-nutrition/formula-feeding/how-much-and-how-often.html
AAP cow's milk: https://www.healthychildren.org/English/healthy-living/nutrition/Pages/Milk-Allergy-Foods-and-Ingredients-to-Avoid.aspx

**AAP contradicts itself at 6 months.** The same page says "6 to 8 ounces (180–240 mL) at each of
4 or 5 feedings in 24 hours" (up to 40 oz / 1200 ml) *and* "Your baby should usually drink no more
than an average of about 32 ounces (960 mL) of formula in 24 hours". 5 × 8 oz = 40 oz > 32 oz.
Implementations must pick one; the 960 ml ceiling is the safer of the two and is the only figure AAP
labels as a limit.

Additional AAP limits and milestones on the same page:
- "If your baby sleeps longer than 4 to 5 hours during the first few weeks after birth and starts
  missing feedings, wake them up and offer a bottle."
- "Between 2 and 4 months of age (or when the baby weighs more than 12 lb. [5.4 kg]), most
  formula-fed babies no longer need a middle-of-the-night feedings."
- "Sometimes patterns of obesity begin during infancy, so it is important not to overfeed."

---

## 3. Age-band table — breast milk

Breastfed volumes are published as *average intake*, never as a schedule, and only two of the five
authorities give any ml/day number at all.

| Age band | Feeds / 24 h | Per feed | Daily total | Source |
|---|---|---|---|---|
| Day 3 | 8–12+ | — | **300–400 ml** | WHO model chapter p. 32 |
| Day 5 | 8–12+ | — | **500–800 ml** | WHO model chapter p. 32 |
| First weeks | **8–12 or more** ("at least 8 to 12 times, or more, every 24 hours"); every 1–3 h in the first few days | — | — | NHS, *Breastfeeding: the first few days* |
| Newborn | 8–12 (AAP: "10-12 sessions in 24 hours is the norm", every 2 h start-to-start) | 1 oz (30 ml) "in the first few weeks" | — | AAP 2024-04-02 |
| 1–6 months | 8–12 (CDC); every 2–4 h | "slowly increases over time" | **≈130 ml/kg/day** | WHO model chapter p. 107 |
| 6–12 months | — | "tops out at roughly 4.5 oz (≈130 ml) at a year of age" (AAP does not write "per feed"; the sentence sits in its breastfed-feeding paragraph, so `[INFERENCE]` per feed) | **no figure published**; breast milk supplies "half or more of a child's energy needs between the ages of 6 and 12 months" | AAP 2024-04-02; WHO fact sheet 2026-08-04 |
| 12–24 months | varies | — | **no figure published**; breast milk supplies "one third of energy needs" | WHO fact sheet 2026-08-04 |

NHS first few days: https://www.nhs.uk/baby/breastfeeding-and-bottle-feeding/breastfeeding/the-first-few-days/
CDC breastfeed: https://cdc.gov/infant-toddler-nutrition/breastfeeding/how-much-and-how-often.html
WHO fact sheet: https://www.who.int/news-room/fact-sheets/detail/infant-and-young-child-feeding

WHO model chapter, p. 32: "On the third day, an infant is usually taking about 300–400 ml per
24 hours, and on the fifth day about 500–800 ml."
WHO model chapter, p. 107: "The full amount of milk normally required by a healthy term baby is
estimated to be 130 ml/kg body weight per day... Divide this into six to 12 feeds depending on the
infant's age and condition."

**Silence to flag:** no AAP, NHS, WHO or CDC page gives a ml/day total for a breastfed baby aged
6–12 months. The AAP's only quantitative statement for that band is a per-feed ceiling of ~4.5 oz
(≈130 ml) at one year. A Daily Target for a breastfed 6–12-month-old cannot be sourced; only a
formula-fed one can.

**NHS is deliberately number-free.** The current NHS bottle-feeding pages state no volumes at all:
"All babies are different. Your baby will know how much milk they need. Some want to feed more
often than others. Just follow your baby's lead." (Page last reviewed 2024-04-02,
https://www.nhs.uk/baby/breastfeeding-and-bottle-feeding/bottle-feeding/advice/). The current
"how much formula" page carries no amounts
(https://www.nhs.uk/baby/breastfeeding-and-bottle-feeding/bottle-feeding/formula-milk-questions/,
last reviewed 2023-09-08). The 150–200 ml/kg/day figure survives in UK clinical guidance (BNFc)
and NHS trust leaflets, **not** on the NHS website itself.

---

## 4. Where breast milk and formula guidance differ

1. **Frequency, not just volume.** AAP: "Breastfed babies usually take smaller, more frequent
   feedings than formula-fed infants." AAP/CDC: 8–12 breastfeeds vs 8–12 formula feeds in the
   newborn period — same count, but breastfed feeds are demand-driven and shorter; formula-fed
   babies "typically feed on a more regular schedule, such as every 3 or 4 hours" (AAP 2022-05-16).
2. **Overfeeding.** AAP: "Infants who are bottle feeding may be more likely to overfeed, because
   drinking from a bottle is faster and may take less effort than breastfeeding" — and the
   bottle-fed baby "may not pull away". NHS: "It's not possible to overfeed a breastfed baby."
   Unicef UK: "Forcing your baby to finish a feed... can mean your baby takes more than they need."
3. **Digestion / what is measured.** Breast milk intake cannot be measured at the breast; both AAP
   and NHS substitute output markers — 6+ wet nappies/day and ≥4 stools/day by day 4 (AAP), "around
   6 wet nappies a day... at least one soft poo" (NHS). Formula intake is directly observable in ml.
4. **Solids at ~6 months.** NHS: introducing solids "should start when your baby is around 6 months
   old"; "They'll still be getting most of their energy and nutrients from breast milk or first
   infant formula" at the start. AAP: "at about 6 months old, solid foods should start to be added".
   WHO: complementary foods from 6 months, breastfeeding continued to 2 years or beyond. All three
   say milk volume stops being the whole story at ~6 months.
5. **Weaning off formula at 12 months.** NHS: "Formula milks are not needed once your baby is
   12 months old. This includes first infant formula, follow-on formula, growing-up milks and
   toddler milks. Cows' milk can be introduced as a main drink from 12 months." CDC: "When your
   toddler is 12 months old, you can switch from infant formula to plain, unsweetened, pasteurized
   whole cow's milk." AAP: 16–24 oz whole milk/day at one year. **Breast milk has no such stop:**
   WHO recommends continued breastfeeding to 2 years or beyond.
6. **Follow-on formula.** Unicef UK: "There is no need for follow-on formula. Stick to a first
   infant formula throughout the first year." NHS agrees: "you do not need to introduce it after
   6 months."
7. **Vitamin D.** CDC: babies fed breast milk exclusively, or <32 oz (960 ml) formula/day, "need
   extra vitamin D, starting shortly after birth" (2026-04-21 and 2026-04-16).

---

## 5. Where authorities conflict or are silent

| Question | Conflict / silence |
|---|---|
| ml/kg/day factor | WHO 130 vs BNFc 150–200 vs AAP ≈163 (with 960 ml cap). No source reconciles them. |
| When the weight-based rule stops applying | **No authority states a stopping age.** BNFc applies 150–200 ml/kg/day across "0–12 months". AAP applies 2½ oz/lb with a 960 ml/24 h ceiling and no age limit. CDC and NHS instead say milk need *falls* as solids rise after 6 months. `[INFERENCE]` The rule is only arithmetically meaningful while milk is the sole food, i.e. before ~6 months; after that, authorities switch to per-feed and frequency guidance and give no daily total. |
| Daily total at 6 months | AAP 720–1200 ml (`[INFERENCE]` from 4–5 × 180–240) vs AAP's own ≤960 ml cap. Unresolved contradiction in one page. |
| Daily total, breastfed 6–12 months | Silent in all five sources. |
| Daily total, breastfed newborn | WHO gives day-3/day-5 totals; AAP/CDC/NHS give none. |
| Feed counts at 6–12 months | AAP "4 or 5 feedings" of milk vs CDC "5 to 6 times in 24 hours" counting formula *or* solids. Not comparable units. |
| Cap on formula | Only AAP states one (960 ml/24 h). WHO, NHS, CDC, Unicef UK state none. |
| Preterm/LBW volumes | Silent in all consumer-facing pages of the five sources (see §6). |
| Mixed breast+formula | Silent. No source gives a combined-volume rule. |

---

## 6. Caveats a generic app must not silently ignore

**Preterm (<37 weeks) and low birth weight (<2500 g).** These infants are explicitly excluded from
the term-infant rules above:

- AAP: "For babies born prematurely or with certain medical conditions, scheduled feedings advised
  by your pediatrician are best" (2024-04-02). AAP also notes `Babies born even a little early can
  be sleepy and may not take in enough in the early weeks.`
- WHO, *Recommendations for care of the preterm or low-birth-weight infant* (2022-11-15,
  https://www.who.int/publications/i/item/9789240058262): mother's own milk first; donor human milk
  when unavailable; "Multicomponent fortification of human milk is not routinely recommended for all
  preterm or LBW infants but may be considered for very preterm (<32 weeks' gestation) or very LBW
  (<1.5 kg) infants"; nutrient-enriched preterm formula when neither milk is available. **The
  guideline gives no ml/kg/day figure** — feeding volumes are set clinically.
- WHO model chapter: babies born small, preterm or sick "may need to be woken to breastfeed at least
  every three hours until they are strong enough to communicate their needs."
- NHS: for preterm babies, "ask your health visitor or GP for advice" on starting solids; the NHS
  premature-baby page gives no volumes.

So: a weight-based ml/kg rule computed by an app is **wrong** for this population — the same kg
gives a different answer, fortification changes energy density, and no cited authority publishes a
consumer-facing number.

**Other hard stops that mean "ask a clinician", not "adjust the target":**
- Weight loss >8–10 % of birth weight in the first days (AAP, 2025-01-13).
- Fewer than 6 wet nappies/day after day 5, or no weight gain (AAP; NHS).
- AAP: consistently wanting more or less than 2½ oz/lb "— discuss it with your pediatrician."
- NHS: "Do not force them to take more milk than they want during a feed. This may be distressing
  for your baby and can lead to overfeeding."

---

## 7. Exact medical-disclaimer wording, quoted

**AAP / HealthyChildren.org** (footer of every article, incl. the two cited above):
> "The information contained on this Web site should not be used as a substitute for the medical
> care and advice of your pediatrician. There may be variations in treatment that your pediatrician
> may recommend based on individual facts and circumstances."

Source: https://www.healthychildren.org/English/ages-stages/baby/formula-feeding/Pages/amount-and-schedule-of-formula-feedings.aspx

AAP also states the substantive limit on any written rule (same site, 2022-05-16):
> "No book―or website―can tell you precisely how much or how often they need to be fed or exactly
> how you should handle them during feedings."

**NHS** (`NHS website` terms and conditions, §4 "Medical information"):
> "4.1 The NHS website provides medical information for use as information or for educational
> purposes. We do not warrant that information we provide will meet your health or medical
> requirements. It is up to you to contact a health professional if you are concerned about your
> health.
> 4.2 The NHS website does not give medical advice in relation to any individual case or patient,
> nor does the NHS website provide medical or diagnostic services."

Source: https://www.nhs.uk/our-policies/terms-and-conditions/

**CDC** (`Website Disclaimers`, last reviewed 2026-02-04):
> "This application is not intended to provide medical advice. Any questions regarding a user's
> personal medical condition should be directed to the application user's primary care physician."

and, under "Information Disclaimer":
> "The information provided using this website is only intended to be general summary information to
> the public. It is not intended to take the place of either the written law or regulations."

Source: https://www.cdc.gov/other/disclaimer.html

**WHO** (`Copyright, permissions and licensing`):
> "WHO provides no warranty of any kind, either expressed or implied. In no event shall WHO be
> liable for damages arising from the use of WHO publications."

Source: https://www.who.int/about/policies/publishing/copyright

**Unicef UK Baby Friendly Initiative** — the 2024 parent leaflet carries **no** medical disclaimer.
Its closest standing statement is:
> "Breastfeeding is the healthiest way to feed your baby. If you decide not to breastfeed or to stop
> breastfeeding, it is possible to restart. Giving infant formula to a breastfed baby will reduce
> your milk supply."

Source: https://www.unicef.org.uk/babyfriendly/wp-content/uploads/sites/2/2019/04/Infant-formula-and-responsive-bottle-feeding.pdf
(leaflet noted as "Updated in 2024": https://www.unicef.org.uk/babyfriendly/baby-friendly-resources/bottle-feeding-resources/infant-formula-responsive-bottle-feeding-guide-for-parents/)

Note the CDC sentence is written for a CDC *app*; it is the only one of the five that speaks about
an application rather than a website or publication.

---

## 8. Compressed rule, as implementable as the sources allow

```
daily_target_ml(age_days, weight_kg, milk_type) — term infants only
  day 1–2      : 15–30 ml per feed × 8–12 feeds         [AAP; CDC]
  day 3        : 300–400 ml                             [WHO]
  day 5        : 500–800 ml                             [WHO]
  week 1       : 30–60 ml per feed × 8–12 feeds         [AAP; CDC]
  month 1      : 90–120 ml per feed, every 3–4 h, up to 960 ml [AAP]
  month 1–6    : 150–200 ml/kg/day  (BNFc)
                 — or 2½ oz/lb ≈ 163 ml/kg/day, ceiling 960 ml (AAP)
                 — or ≈130 ml/kg/day (WHO, breast milk average)
  month 6–12   : 180–240 ml per feed × 4–5 feeds (AAP) — but AAP caps at 960 ml
                 CDC: intake falls as solids rise; 5–6 feeds/day incl. solids
                 no ml/day figure exists for breastfed infants in this band
  month 12+    : formula ends; whole cow's milk 470–710 ml/day (AAP/CDC)
                 breast milk continues (WHO: to 2 years or beyond)

reject / require clinician-set target if:
  gestational_age < 37 weeks, birth_weight < 2500 g, or baby is sick
  → AAP: "scheduled feedings advised by your pediatrician"; WHO 2022 gives no ml figure
```

**The number an app cannot get from the literature:** a defensible single Daily Target for a
breastfed baby at 6–12 months, and any target at all for a preterm/LBW baby.

## 9. Source inventory

| Source | URL | Version / date fetched |
|---|---|---|
| AAP, *Amount and Schedule of Baby Formula Feedings* | https://www.healthychildren.org/English/ages-stages/baby/formula-feeding/Pages/amount-and-schedule-of-formula-feedings.aspx | Last Updated 2022-05-16; from *Caring for Your Baby and Young Child*, 7th ed. (2019) |
| AAP, *How Often and How Much Should Your Baby Eat?* | https://www.healthychildren.org/English/ages-stages/baby/feeding-nutrition/Pages/how-often-and-how-much-should-your-baby-eat.aspx | Last Updated 2024-04-02 |
| AAP, *How to Tell if Your Breastfed Baby is Getting Enough Milk* | https://www.healthychildren.org/English/ages-stages/baby/breastfeeding/Pages/How-to-Tell-if-Baby-is-Getting-Enough-Milk.aspx | Last Updated 2025-01-13 |
| AAP, *Cow's Milk Alternatives: Parent FAQs* | https://www.healthychildren.org/English/healthy-living/nutrition/Pages/Milk-Allergy-Foods-and-Ingredients-to-Avoid.aspx | Last Updated 2022-06-02 |
| CDC, *How Much and How Often to Feed Infant Formula* | https://cdc.gov/infant-toddler-nutrition/formula-feeding/how-much-and-how-often.html | Last reviewed 2026-04-21 |
| CDC, *How Much and How Often to Breastfeed* | https://cdc.gov/infant-toddler-nutrition/breastfeeding/how-much-and-how-often.html | Last reviewed 2026-04-16 |
| CDC, *Website Disclaimers* | https://www.cdc.gov/other/disclaimer.html | Last reviewed 2026-02-04 |
| NHS, *Bottle feeding advice* | https://www.nhs.uk/baby/breastfeeding-and-bottle-feeding/bottle-feeding/advice/ | Last reviewed 2024-04-02 |
| NHS, *Formula milk: common questions* | https://www.nhs.uk/baby/breastfeeding-and-bottle-feeding/bottle-feeding/formula-milk-questions/ | Last reviewed 2023-09-08 |
| NHS, *Breastfeeding: the first few days* | https://www.nhs.uk/baby/breastfeeding-and-bottle-feeding/breastfeeding/the-first-few-days/ | accessed 2026-10-22 |
| NHS, *Drinks and cups for babies and young children* | https://www.nhs.uk/baby/weaning-and-feeding/drinks-and-cups-for-babies-and-young-children/ | Last reviewed 2022-10-05 (review due 2025-10-05 — overdue) |
| NHS, *Your baby's first solid foods* | https://www.nhs.uk/baby/weaning-and-feeding/babys-first-solid-foods/ | accessed 2026-10-22 |
| NHS, *NHS website terms and conditions* §4 | https://www.nhs.uk/our-policies/terms-and-conditions/ | accessed 2026-10-22 |
| BNF for Children (NICE), *Enteral nutrition* | https://bnfc.nice.org.uk/treatment-summaries/enteral-nutrition/ | accessed 2026-10-22 |
| WHO, *Infant and young child feeding: model chapter*, 2nd ed. | https://iris.who.int/server/api/core/bitstreams/2966cbcb-d5a0-4ee5-b39f-4dc1bde543ba/content | 2nd ed., © WHO 2025, ISBN 978-92-4-011373-2, 168 pp. |
| WHO, *Infant and young child feeding* fact sheet | https://www.who.int/news-room/fact-sheets/detail/infant-and-young-child-feeding | 2026-08-04 |
| WHO, *Recommendations for care of the preterm or low-birth-weight infant* | https://www.who.int/publications/i/item/9789240058262 | 2022-11-15 |
| WHO, *Copyright, permissions and licensing* | https://www.who.int/about/policies/publishing/copyright | accessed 2026-10-22 |
| Unicef UK Baby Friendly, *Infant formula and responsive bottle feeding* | https://www.unicef.org.uk/babyfriendly/wp-content/uploads/sites/2/2019/04/Infant-formula-and-responsive-bottle-feeding.pdf | leaflet updated 2024 |
