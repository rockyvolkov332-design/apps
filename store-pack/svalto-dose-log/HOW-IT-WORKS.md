# Svalto Dose Log: how the app works

An IVF / egg-freezing injection organiser. It records what the clinic says (meds, doses, times, dose changes), reminds the user, tracks injection sites and the cycle timeline, and can read a photo of the clinic's instruction sheet **on the device**. It never calculates or suggests a dose. Flutter, iOS only for now. There's no backend: all data is on the device.

## Run it locally

```bash
flutter pub get
flutter test                       # logic + scan-parsing tests
flutter run                        # real first-run flow
flutter run --dart-define=DEMO=true --dart-define=TAB=0   # seeded demo cycle
```

- Flutter 3.47, Xcode 26, iPhone only (`TARGETED_DEVICE_FAMILY = 1`), team 245BMKS579, bundle `com.svalto.doselog`, version 1.0.0+2.
- The scan feature's native code is in `ios/Runner/AppDelegate.swift`. It needs iOS 26 + Apple Intelligence for the AI path; otherwise a text-matching fallback runs.

**Demo flags** (debug only):
- `DEMO=true` seeds an IVF cycle on day 6 with Gonal-f, Menopur, Cetrotide and a trigger shot tomorrow
- `TAB=0..3` picks Today / Meds / Sites / Cycle
- `SCREEN=paywall[:reason] | onboarding | empty | scan`
- `FREE=true` shows the free plan

⚠️ Debug builds show a "Debug: unlock Pro" button on the paywall. It isn't in release builds, but don't use debug screenshots of the paywall in the store.

## Code map (`lib/`)

| File | What it does |
|---|---|
| `models.dart` | `Cycle` (type, day 1, clinic, trigger/retrieval/transfer dates), `Med` (name, unit, times, mix/cold flags, `DoseChange` history), `DoseLog` (taken at, amount, site), `MedPreset` (11 IVF med names + units, never doses) |
| `store.dart` | `AppState` persisted with SharedPreferences. Free limits: 2 meds, 1 scan. `dosesOn(day)`, site rotation helpers |
| `notify.dart` | Dose reminders 3 days ahead (iOS 64-notification cap) + 5 trigger-shot reminders (24 h, 2 h, 30 min, 10 min, on the minute), rescheduled on every change |
| `scan.dart` | Scan flow: image → native `svalto/sheet` MethodChannel → text → structured lines → matched to meds as Change / New / Same |
| `purchases.dart` | Apple IAP: cycle pass (60 days, non-renewing), monthly, yearly |
| `ui.dart` | Svalto design system (off-white, ink, Instrument Sans) |
| `screens/` | `onboarding`, `today`, `meds`, `sites`, `cycle`, `scan_review`, `paywall` |

## User flow

1. **Onboarding:** cycle type (IVF / egg freezing / frozen transfer) → day 1 of meds → clinic name → notifications → paywall (closable).
2. **Today:** every dose for the day by time. Tap to log with an injection-site picker. Mix/fridge tags. Browse other days. "Clinic changed a dose?" flow. Trigger-shot live countdown.
3. **Meds:** add from presets or custom (free: 2). Every dose change is kept as history (e.g. 225 → 187.5 IU).
4. **Sites:** body map coloured by how recently each site was used, plus the site used longest ago.
5. **Cycle:** timeline with "tomorrow" / "in 3 days" chips.
6. **Scan** (free: 1, Pro: unlimited): take a photo or pick a screenshot. Apple Vision reads the text; on iOS 26 with Apple Intelligence, on-device Foundation Models turns it into med / dose / unit / time. The review screen marks each line Change / New / Same, with a banner to compare against the sheet. Nothing saves until "Save to my schedule". Lab values (pg/mL, follicle mm) are skipped.

## Billing

`com.svalto.doselog.cyclepass` ($29.99, non-renewing, 60 days), `com.svalto.doselog.pro.monthly` ($14.99), `com.svalto.doselog.pro.yearly` ($49.99). The cycle pass sets `proUntil` = purchase date + 60 days; the subscriptions set `pro`.

⚠️ There's no launch-time entitlement re-check yet (Halve has one): a lapsed subscription stays Pro.

## Content rules

- Never calculate, suggest or change doses: the app only records what the clinic said.
- No medical claims.
- Individual developer account. Keep the category Health & Fitness (guideline 5.1.1(ix) risk for Medical).
