# Store uploads

TestFlight and Play internal run from [`.github/workflows/deploy-mobile.yml`](../../.github/workflows/deploy-mobile.yml) when a push to `main` changes `memory_book` (markdown and `memory_book/docs` do not count). A website-only change does not upload a build.

The lanes live in `memory_book/fastlane/Fastfile`. They call `flutter build ipa` and `flutter build appbundle`, then upload. CI uses the organization signing secrets already shared with Arogya. It does not submit the App Store version for review and it does not roll Play production.

On CI the build number is the higher of `pubspec.yaml` and one more than the newest build already on that store. The workflow does not commit that number.

Before the iOS archive, CI installs CocoaPods 1.17.0 with the job's Ruby and runs the archive outside the Fastlane bundle. The sign-in plugin still needs `pod`. The copy preinstalled on the Mac runner was built for a different Ruby, and Bundler rejects a `pod` that is not in the Fastlane Gemfile.

`fastlane/whats_new.txt` is sent with the build. TestFlight shows it as What to Test. Play internal shows it as the release notes, which must stay within 500 characters. The public App Store What's New stays on the version page in App Store Connect.

Local checks, from `memory_book`:

```bash
bundle install
bundle exec fastlane ios beta
bundle exec fastlane android beta
```
