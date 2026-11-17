# Svalto Dose Log: store listing content

App Store Connect record: **Svalto Dose Log: IVF Tracker**, App Store record 6835472823, bundle ID `com.svalto.doselog`. iOS only for now; there's no Android build yet.

## Rules for this listing

- **Never suggest or calculate doses.** The app only records what the clinic says.
- **No medical claims.** Don't say "improves success rates", "treats" or "diagnoses".
- **No drug brand names** in the subtitle or keywords (Gonal-f, Menopur and so on are trademarks). The app may show med names the user types.

## App Store: product page (English U.S.)

| Field | Content |
|---|---|
| Name (30) | Svalto Dose Log: IVF Tracker |
| Subtitle (30) | IVF injection schedule & log |
| Primary category | Health & Fitness (safer than Medical for an individual developer: guideline 5.1.1(ix)) |
| Secondary category | Medical |
| Price | Free with in-app purchases |
| Support URL | https://rockyvolkov332-design.github.io/apps/svalto-dose-log/support.html |
| Marketing URL | https://rockyvolkov332-design.github.io/apps/svalto-dose-log/ |
| Privacy Policy URL | https://rockyvolkov332-design.github.io/apps/svalto-dose-log/privacy.html |
| Version | 1.0 |
| Copyright | © 2026 Rokki Volkov |

**Promotional text (170):**
> Every injection, every dose change, every reminder for your IVF or egg-freezing cycle. Photograph your clinic's instructions and check them into your schedule.

**Keywords (100):**
```
ivf tracker,injections,fertility,egg freezing,trigger shot,stims,medication log,cycle,transfer
```

**Description (4,000):**
> Your IVF cycle, handled.
>
> Svalto Dose Log keeps every injection in one calm place: what to take tonight, at what time, and where you injected last. It records what your clinic tells you. It never suggests or changes a dose.
>
> TODAY, IN ORDER
> Every dose for the day sorted by time. Tap to log it and pick the injection site. Mix and fridge tags so nothing gets missed.
>
> EVERY DOSE CHANGE, ON RECORD
> Clinics change doses after scans. Update a med and the full history stays, so you can show your nurse exactly what you took and when.
>
> SCAN YOUR CLINIC'S INSTRUCTIONS
> Photograph the instruction sheet or take a screenshot of the portal message. The text is read on your phone, matched to your meds, and shown line by line so you can check it against the sheet before anything is saved.
>
> TRIGGER-SHOT COUNTDOWN
> A live countdown, plus reminders 24 hours, 2 hours, 30 minutes and 10 minutes before, and at the exact time.
>
> INJECTION-SITE ROTATION
> A body map shows where you injected and when, and the site you used longest ago.
>
> YOUR CYCLE ON ONE TIMELINE
> Meds start, trigger shot, retrieval, transfer and blood test, with "tomorrow" and "in 3 days" at a glance.
>
> PRIVATE BY DESIGN
> No account and no servers. Your cycle, meds and logs stay on your phone. Scans are read on the device and never uploaded.
>
> DOSE LOG PRO
> The free plan holds two medications and one scan. Pro unlocks every medication, unlimited scans, trigger-shot reminders and the site map.
> • Cycle pass: one payment for 60 days, does not renew.
> • Monthly or yearly auto-renewing subscriptions, charged to your Apple ID. They renew unless cancelled at least 24 hours before the period ends. Manage them in Settings › Apple ID › Subscriptions.
>
> Svalto Dose Log is an organiser. It does not give medical advice. Always follow your clinic's instructions.
>
> Terms of use (Apple standard EULA): https://www.apple.com/legal/internet-services/itunes/dev/stdeula/
> Privacy: https://rockyvolkov332-design.github.io/apps/svalto-dose-log/privacy.html

**Screenshots (6.9", 1320 × 2868):** `appstore-6.9/01-today.png` … `05-scan.png`, in that order. Unframed originals are in `raw/`. No paywall screenshot yet: the debug build shows a "Debug: unlock Pro" button, so the subscription review screenshot has to come from a TestFlight/release build.

## App Store: App Privacy

**Data Not Collected.** There is no account, server, analytics or ads. Camera and photos are used only to read the instruction sheet on the device (Apple Vision + on-device Apple Intelligence), and nothing is uploaded.

## App Store: age rating

"Medical/Treatment Information" = **Infrequent/Mild**, everything else None. That likely gives 12+; confirm what the questionnaire shows.

## App Store: App Review information

- **Sign-in required:** No. Untick the box.
- **Contact:** Rokki Volkov · +64 22 646 3087 · hautelimited332@gmail.com
- **Notes:**
> Svalto Dose Log is a medication schedule and injection log for people going through IVF or egg freezing. It records what the user's clinic tells them; it never calculates, suggests or changes doses. There is no sign-in and no server; all data is stored on the device.
> First launch: choose cycle type and day 1, enter a clinic name, allow notifications, then the paywall appears. Close it to use the free plan (two medications, one scan).
> "Scan new instructions" (Today screen) reads a photo or screenshot of the clinic's instruction sheet entirely on the device: Apple Vision for text, and on iOS 26 devices with Apple Intelligence, the on-device Foundation Models framework to structure it. Without Apple Intelligence, a text-matching fallback runs. Nothing is uploaded. The user reviews every line before saving.
> In-app purchases: com.svalto.doselog.cyclepass ($29.99, non-renewing, 60 days), com.svalto.doselog.pro.monthly ($14.99/month), com.svalto.doselog.pro.yearly ($49.99/year). Restore is on the paywall.

## In-app purchases

| Product ID | Type | Price | Display name | Description |
|---|---|---|---|---|
| com.svalto.doselog.cyclepass | Non-renewing subscription, 60 days | $29.99 | Cycle Pass | Dose Log Pro for one 60-day cycle |
| com.svalto.doselog.pro.monthly | Auto-renewing, 1 month | $14.99 | Dose Log Pro Monthly | Every med, unlimited scans, trigger reminders |
| com.svalto.doselog.pro.yearly | Auto-renewing, 1 year | $49.99 | Dose Log Pro Yearly | Every med, unlimited scans, trigger reminders |

⚠️ The research doc proposed $12.99/month; the code says $14.99. Confirm with Damian before creating the monthly product.
⚠️ The in-app Privacy button still opens svalto.com/privacy-policy (the store's policy). Point it to the app page above before App Review.
