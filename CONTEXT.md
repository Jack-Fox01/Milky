# Milky

A single-caregiver tracker for how much milk a baby drinks. The caregiver logs bottle feeds; a bottle on screen fills toward a daily target and celebrates when it is reached.

## Language

**Baby**:
The one infant whose intake this device tracks.
_Avoid_: Child, patient, infant

**Caregiver**:
The single person operating Milky on one device, with no account.
_Avoid_: User, mother, parent, account

**Feed**:
One bottle feeding: a volume, a Milk Type, and a timestamp.
_Avoid_: Bottle, entry, intake

**Milk Type**:
What was in the bottle: breast milk or formula.
_Avoid_: Kind, category

**Bottle**:
The on-screen vessel that accumulates the Day's Feeds and fills toward the Daily Target.
_Avoid_: Gauge, jar, tracker

**Daily Target**:
The amount of milk the Baby is expected to drink in one Day, suggested from age and editable by the Caregiver.
_Avoid_: Goal, quota

**Day**:
The local calendar day, 00:00 to 24:00, at which boundary the Bottle resets.
_Avoid_: Period, window, session

**Pump Session**:
An expressed-milk event logged on its own; it never fills the Bottle or counts toward the Daily Target.
_Avoid_: Pump, expression, output

**History**:
Past Days' Feeds, browsable and editable.
_Avoid_: Log, archive

**Reminder**:
A nudge that a Feed is overdue.
_Avoid_: Alert, notification
