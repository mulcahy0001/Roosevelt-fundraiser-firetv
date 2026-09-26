# Roosevelt Fundraiser Display

A minimal Fire TV/Android TV WebView app that opens the Roosevelt fundraiser display full-screen:

`https://roosevelt-hermit-crabs-pledges.mulcahy001.chatgpt.site/`

## Behavior and remote controls

- The screen stays awake and Android system bars are hidden.
- **Menu** refreshes the display. A long press on **Play/Pause** is an alternate refresh action.
- **Back** navigates WebView history when available. At the home page, press Back twice within two seconds to exit.
- A failed main-page load retries after 3, 6, 12, then 30 seconds (continuing every 30 seconds). Reconnecting to a network triggers an immediate retry on Fire OS 7+.
- Long-press **Rewind** to toggle optional launch-after-boot. It is off by default. A toast confirms the setting.
- Only HTTPS top-level navigation to the configured fundraiser host is allowed. Subresources may load from other HTTPS hosts. Cleartext HTTP is disabled.

## Build in Android Studio

1. Install a current stable Android Studio and Android SDK Platform 35.
2. Open this `RooseveltFundraiserDisplay` folder (not only the `app` folder).
3. Let Android Studio install/sync the requested Android Gradle Plugin and SDK components.
4. Choose **Build → Build Bundle(s) / APK(s) → Build APK(s)**.
5. The debug APK will be at `app/build/outputs/apk/debug/app-debug.apk`.

Or, with JDK 17 and Android SDK 35 configured, build from a terminal:

```sh
./gradlew assembleDebug
```

This project uses only Android platform APIs. It has no Play Services or third-party runtime dependencies. Minimum SDK 22 covers Fire OS 5 and newer Android-based Fire TV devices; actual page compatibility also depends on the device's installed WebView version.

## Sideload to a Fire Stick with ADB

1. On Fire TV, open **Settings → My Fire TV → About**, select the device name seven times if Developer Options is hidden, then enable **ADB Debugging** and **Install unknown apps** where shown.
2. Find the Fire Stick IP under **Settings → My Fire TV → About → Network**.
3. On a computer with Android Platform Tools, run:

   ```sh
   adb connect FIRE_STICK_IP:5555
   adb install -r app/build/outputs/apk/debug/app-debug.apk
   ```

4. Accept the debugging prompt on the TV. Open **Roosevelt Fundraiser Display** from Apps.

To launch it from the computer for testing:

```sh
adb shell am start -n com.roosevelt.fundraiserdisplay/.MainActivity
```

To remove it:

```sh
adb uninstall com.roosevelt.fundraiserdisplay
```

## Launch-after-boot note

Long-press Rewind while the app is open to enable or disable auto-start. Amazon may restrict apps from opening a foreground activity after boot on some recent Fire OS releases. That failure is intentionally ignored, and the normal Fire TV launcher icon always remains available. For event use, test a full power cycle on the exact Fire Stick beforehand.
