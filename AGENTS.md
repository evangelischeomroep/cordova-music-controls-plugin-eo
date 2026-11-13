# cordova-music-controls-plugin-eo

The EO's fork of the public Cordova Music Controls plugin: it shows media controls in the Android notification, the iOS lock screen and control center, and on Windows, and reports button and headset events to the app. Outside users read its README, so that stays written for them.

## Ways to hurt yourself

1. **Bumping one version.** `package.json` and `plugin.xml` both carry the version. Change both in the same commit.
2. **A native file that never ships.** Cordova copies only the source files `plugin.xml` lists per platform. When you add, rename or remove a file under `src/`, update its `<source-file>` or `<header-file>` entry.

## Running locally

There is no build step. To try a change, add the plugin to a Cordova app from your clone with `cordova plugin add <path-to-clone>` and run that app on the platform you changed.

## Done means green

There are no scripts, tests or CI. A change is done when the app above builds and the controls and events work on every platform you touched.

## Documentation

Most changes need no documentation; agents can read the code.

- The README is for the plugin's users: keep its methods, options and event names current when the API changes.
- A reason that belongs to one spot in the code goes in a short comment there.

## How it works

`www/MusicControls.js` is the JavaScript API the app gets as the global `MusicControls`. It fills in defaults and calls the native `MusicControls` service through `cordova.exec`. Events flow back through one pending `watch` callback: native code answers it with the event, and the JavaScript side passes the event to the subscribed function and registers a new `watch`. On Android, `MusicControlsNotification` builds the notification on its own channel, and broadcast receivers turn its buttons and headset intents into events. On iOS, `MusicControls.m` sets the now-playing info and handles `MPRemoteCommandCenter` commands. On Windows, `MusicControlsProxy.js` implements the service in JavaScript.

## Where code lives

- `www/` - the JavaScript API.
- `src/android/`, `src/ios/`, `src/windows/` - the native side per platform.
- `plugin.xml` - what Cordova installs per platform: source files, permissions and the Android service.
