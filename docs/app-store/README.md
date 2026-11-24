# Mac App Store submission kit

Everything App Store Connect asks for, ready to paste or to push with fastlane. The format matches Sizer's, so every Perfect Ink app keeps its listing the same way.

| What | Where |
|---|---|
| Listing text (name, subtitle, promo, description, keywords, URLs, What's New), copyright, categories, App Review notes and contact | `listing.yml` |
| Screenshots, 2880×1800 (1 to 10) | `screenshots/en-US/`, in file-name order. Until there are any, fastlane leaves App Store Connect's screenshots alone |
| Privacy policy | `../PRIVACY.md`, served at `privacy_url` in `listing.yml` |
| Automation | `../../fastlane/Fastfile`, `../../.github/workflows/appstore.yml`, `make appstore`, `make metadata`, `make submit` |

## Fixed answers

| App Store Connect field | Value |
|---|---|
| Bundle ID | org.perfectink.rolodex (fixed once the record exists) |
| Team | XS9W9A8FWM |
| SKU | `rolodex-mac` |
| Primary language | English (U.S.) |
| Categories | Productivity, then Business |
| Age rating | every question "None" / "No", result 4+ |
| App Privacy | Data Not Collected. Addresses go to Apple Maps only when the map is open, which Apple doesn't count as collection by the app |
| Sign-in required | No |
| Export compliance | not asked: `ITSAppUsesNonExemptEncryption` is false in Info.plist |
| Content rights | contains no third-party content |
| Version | matches the release tag, e.g. 1.0.0 |
| Build | the App Store workflow's run number |

## One-time setup the API cannot do

1. **App ID**: developer.apple.com → Identifiers, `org.perfectink.rolodex` under team XS9W9A8FWM. The sandbox and Contacts entitlements need nothing extra here.
2. **API key**: App Store Connect → Users and Access → Integrations, a team key with the App Manager role. Put it in your secrets file and run `make secrets` in perfectink.org. Xcode creates the signing certificate and profile through this key, so no .p12 is needed.
3. **App record**: App Store Connect → Apps → +, platform macOS, the name from `listing.yml`, bundle ID `org.perfectink.rolodex`, SKU `rolodex-mac`.
4. **App Privacy**: "Data Not Collected", then Publish.
5. **Age rating**: answer the questionnaire (all None / No).

After that, publishing a GitHub release (not a pre-release) builds, uploads, pushes the listing and submits for review.
