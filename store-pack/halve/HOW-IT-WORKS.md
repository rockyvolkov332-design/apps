# Halve: how the app works

Halve helps adults quit nicotine pouches by **tapering the strength** (halving the mg each step) on a schedule they set. Flutter, iOS + Android, one codebase. There's no backend: all data is on the device.

## Run it locally

```bash
flutter pub get
flutter test                       # 16 logic tests (taper maths, stats, reminders, paywall maths)
flutter run                        # real first-run flow
flutter run --dart-define=DEMO=true --dart-define=TAB=0   # seeded demo user (screenshots)
```

- Flutter 3.47 / Dart 3.13, Xcode 26, Android SDK 36 + platform 37 (compileSdk 37), Java 17.
- iOS: iPhone only (`TARGETED_DEVICE_FAMILY = 1`), team 245BMKS579, bundle `com.halveapp.quit`.
- Android: applicationId `com.halveapp.quit`. Release signing reads `android/key.properties`, which is **not in the repo**. Ask Damian for the upload key when you need to upload. Without it, release builds fall back to the debug key.

**Demo flags** (debug only):
- `DEMO=true` seeds a user 19 days in (6 mg, 10/day, now on step 2 at 3 mg)
- `TAB=0..3` picks the tab
- `SCREEN=paywall | paywall:step | sos | sos:delay | settings | privacy | craving-sheet | log-sheet | onboarding:0..4`
- `FREE=true` shows the free plan

The demo shifts the app clock to 3:42 pm so the seeded day reads naturally.

## Code map (`lib/`)

| File | What it does |
|---|---|
| `brand.dart` | Name, bundle ID, Terms/subscription URLs. Rename the app here |
| `models.dart` | Pure logic: `Habit`, `TaperStep`, `Plan` (step for a date, days left, nicotine-free date), `PouchLog`, `CravingLog`, `Stats` (streak, money/mg saved, craving patterns), `defaultLadder()` (6→3→1.5→0) |
| `store.dart` | `AppState` (ChangeNotifier) persisted with SharedPreferences: habit, plan, logs, reminders, reasons, `pro` flag |
| `notify.dart` | `plannedReminders()` (pure, tested) + `Notifier`, which schedules a 14-day rolling window of local notifications |
| `purchases.dart` | Store billing via `in_app_purchase`: products, buy, restore, launch entitlement check |
| `demo.dart` | Seeded demo data for screenshots |
| `ui.dart` | Design system: palette (ink / bone / chartreuse / coral), Bricolage Grotesque, shared widgets, the half-circle mark |
| `screens/` | `onboarding`, `today`, `plan`, `cravings`, `progress`, `sos`, `paywall`, `settings` (+ in-app Privacy and Terms) |

## User flow

1. **Onboarding:** confirm 18+ → current strength, pouches a day, can price and size → reasons for quitting → plan preview (step length, start date) → reminder times + notification permission → paywall (closable).
2. **Today:** one tap logs a pouch at today's step strength; hold to pick a different strength. Ring = today's count vs limit. Shows saved money, on-target streak, days to nicotine-free.
3. **Plan:** all steps with dates. Pro can edit each step's mg / days / daily limit.
4. **Cravings:** log strength, trigger and outcome. Pro sees patterns; free users see their top trigger once they have 3+ cravings.
5. **Progress:** money and mg skipped vs the old habit, streaks, 14-day chart.
6. **SOS** (free, always): breathing circle (4-4-6) or 5/10/15-minute delay timer, plus the user's own reasons.

## Free vs Pro

| Free | Pro (weekly $7.99 / yearly $49.99) |
|---|---|
| Counter, money saved, streak, craving log, SOS | Steps 2+ of the taper (the step-down itself) |
| Step 1 of the plan | Plan editing |
| Daily reminders (morning limit, evening check-in) + one "step 2 is ready" nudge | Step-change heads-ups |
| Top trigger (after 3 cravings) | Full craving patterns |

When step 1 ends, a free user sees a locked "Step 2 is ready: 3 mg" card, which opens a reason-specific paywall.

## Billing

- Product IDs (same on both stores): `com.halveapp.quit.pro.weekly`, `com.halveapp.quit.pro.yearly`. iOS: one subscription group "Halve Pro" (ID 22587513). Play: two subscriptions, each with one auto-renewing base plan.
- Purchases unlock Pro via the purchase stream. At every launch, `syncEntitlement()` re-checks the store:
  - iOS: StoreKit 2 `currentEntitlements`
  - Android: `queryPastPurchases`

  A lapsed subscription goes back to free; an active one is restored without tapping Restore. Android purchases are acknowledged with `completePurchase`, or Play refunds them after 3 days.
- On Android the paywall prices the base plan, never a trial offer.

## Reminders

`plannedReminders(plan, reminders, now, pro:)` builds the list, and `Notifier.reschedule` hands it to the OS on every change and every launch. That's 14 days ahead, max 60 (iOS caps pending notifications at 64). Android uses the "Daily plan" channel, `inexactAllowWhileIdle`, and a boot receiver that re-schedules after a restart.

## Content rules (store + app)

- Never show or glamorise a pouch or can.
- No medical claims ("habit tracker, not treatment").
- 18+ only.
- No other brands' trademarks in store metadata.
