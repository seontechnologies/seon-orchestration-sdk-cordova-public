# seon-orchestration-cordova

[![iOS](https://img.shields.io/badge/iOS-15%2B-blue.svg)](https://www.apple.com/ios/)
[![Android](https://img.shields.io/badge/Android-SDK%2026%2B-green.svg)](https://developer.android.com/about/versions/oreo)
[![Cordova](https://img.shields.io/badge/Cordova-10%2B-blueviolet.svg)](https://cordova.apache.org/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

Cordova plugin for the [SEON Orchestration SDK](https://seon.io), providing identity verification flows for iOS and Android.

## Which repository is this?

| Clone / remote | What you have at the repo root |
|----------------|--------------------------------|
| **Development** clone of `seon-orchestration-cordova` | Cordova **plugin** sources (`plugin.xml`, `src/`, `www/`, `tests/`, …) **and** [`examples/`](examples/) |
| **Public** GitHub [`seon-orchestration-cordova`](https://github.com/seontechnologies/seon-orchestration-cordova) (sample-only checkout) | **Only** [`examples/cordova`](examples/cordova) and [`examples/ionic`](examples/ionic) — no standalone plugin package at the root |

## Installation

### Cordova

From npm (when published):

```sh
cordova plugin add cordova-plugin-seon-orchestration
```

From a local clone of the **full plugin** repository:

```sh
cordova plugin add /path/to/seon-orchestration-cordova
```

From the **public samples** repository, install the bundled tarball (under `examples/cordova`):

```sh
cd examples/cordova
cordova plugin add ./cordova-plugin-seon-orchestration-1.0.2.tgz
```

For iOS, install pods after adding the plugin:

```sh
cd platforms/ios && pod install
```

### Ionic / Capacitor

```sh
npm install cordova-plugin-seon-orchestration@^1.0.2
npx cap sync
```

## Sample apps

Runnable samples live under [`examples/cordova`](examples/cordova) and [`examples/ionic`](examples/ionic).

**Cordova — device and signing:** copy `examples/cordova/build.json.example` to `examples/cordova/build.json`, set your Apple `developmentTeam` ID, then from `examples/cordova` run `npm run ios:signed` (or pass `--buildConfig=build.json` to `cordova run ios --device`). Run `npx cordova prepare ios` after changes so the `after_prepare` hooks can refresh native projects and run `pod install`. Use `npm run ios` from `examples/cordova` (uses local `cordova` and `ios-deploy`). If `cordova run` installs the app but `ios-deploy` exits with a DeveloperDiskImage error, the build may still have succeeded—open the app on the device or update Xcode DeviceSupport.

**Ionic:** from `examples/ionic`, run `npm install`, `npx ionic build`, `npx cap sync ios`, then open `ios/App/App.xcworkspace` in Xcode or use `npx cap open ios`.

## Quick start

```javascript
document.addEventListener('deviceready', async function () {
  await SeonOrchestration.initialize({
    baseUrl: 'https://api.seon.io/orchestration-api',
    token: 'your-session-token',
    language: 'en', // optional
  });

  const result = await SeonOrchestration.startVerification();

  switch (result.status) {
    case SeonOrchestration.SeonVerificationStatus.completedSuccess:
      console.log('Verification passed!');
      break;
    case SeonOrchestration.SeonVerificationStatus.completedFailed:
      console.log('Verification failed.');
      break;
    case SeonOrchestration.SeonVerificationStatus.interruptedByUser:
      console.log('User cancelled.');
      break;
    case SeonOrchestration.SeonVerificationStatus.error:
      console.log('Error:', result.errorMessage);
      break;
    default:
      break;
  }

  await SeonOrchestration.destroy();
});
```

### Custom theme

`theme` is a JSON string for native UI customization. See [SEON workflow initialization](https://docs.seon.io) for the schema.

```javascript
await SeonOrchestration.initialize({
  baseUrl: 'https://api.seon.io/orchestration-api',
  token: 'your-session-token',
  theme: JSON.stringify({ /* your theme config */ }),
});
```

## API overview

| Method | Description |
|--------|-------------|
| `SeonOrchestration.initialize(config)` | Initialize with base URL, token, optional language/theme |
| `SeonOrchestration.startVerification()` | Present verification UI and return the result |
| `SeonOrchestration.destroy()` | Release SDK resources |

| Type | Description |
|------|-------------|
| `SeonVerificationStatus` | Verification outcomes |
| `SeonErrorCode` | SDK error codes |

## Permissions

### iOS

The plugin adds common `Info.plist` usage descriptions (camera, microphone, photo library, optional location).

### Android

Required permissions are merged from the plugin’s `plugin.xml`.

## Platform requirements

| Platform | Minimum |
|----------|---------|
| iOS | 15.0+ (see example `config.xml` `deployment-target`; aligns with the bundled native SDK / sample apps) |
| Android | API 26+ (see example `config.xml` `android-minSdkVersion` and Ionic `capacitor.config.json`) |
| Cordova CLI | 10+ (`plugin.xml` engines) |
| cordova-ios | 6+ |
| cordova-android | 10+ |

## Documentation

In-repo docs (Mintlify) live under [`docs/`](docs/). Run locally:

```sh
npm install -g mintlify
cd docs
mintlify dev
```

## License

MIT

## Changelog

### 1.0.2

- Updated native iOS `SEONOrchSDK` to 1.0.3
- Updated native Android `orchestration-android-sdk` to 1.0.1

### 1.0.1

- Updated native iOS `SEONOrchSDK` to 1.0.2

### 0.1.0

Initial version.
