# App Store Connect: which folder goes in which slot (both apps)

Every folder exists for both apps: `halve/` and `svalto-dose-log/`. Upload the 5 images in name order (01 to 05).

| App Store Connect slot | Accepted sizes | Folder |
|---|---|---|
| iPhone with Dynamic Island (medium display, 6.1"/6.3") | 1206 × 2622 | `appstore-6.3/` |
| iPhone with Dynamic Island (large display, 6.9"), if shown | 1320 × 2868 | `appstore-6.9/` |
| iPhone Duo, inner display | 2007 × 2853 | `appstore-duo-inner/` |
| iPhone Duo, outer display | 1398 × 2034 | `appstore-duo-outer/` |
| Header and Search Results (iOS 27+, optional) | universal 16:9, 5244 × 2950 | `creative-assets/universal-5244x2950.png`, then tick "Use header asset in search results" |
| Or separately: product page header | 21:9, 3840 × 1646 | `creative-assets/header-3840x1646.png` |
| Or separately: search results | 3:2, 3840 × 2560 | `creative-assets/search-3840x2560.png` |
| iPad 13" | n/a | Not needed: both builds are iPhone-only (TARGETED_DEVICE_FAMILY = 1). App Store Connect only requires iPad screenshots when the binary supports iPad, and iPad screenshots of an iPhone app risk rejection under guideline 2.3.3. |
| Apple Watch | n/a | Not needed: neither app has a watch app. |

The "File dimensions are invalid" error came from dropping the 6.9" files (1320 × 2868) into the 6.3" slot. Use `appstore-6.3/` there.

All images are RGB PNG with no alpha. All key art in the creative assets is inside Apple's safe area (header 1646 × 661, search 2168 × 1030, universal 1402 × 962).
