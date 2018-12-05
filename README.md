# Reproducing Cordova Plugin Whitelist issue 32

This is a minimal cordova app to reproduce a bug with the `cordova-plugin-whitelist` reported here: https://github.com/apache/cordova-plugin-whitelist/issues/32

This app basically whitelists a URL using a wildcard on the path, and attempts
to open the root path **in the app**.

```xml
<allow-navigation href="https://github.com/apache/cordova-plugin-whitelist/*" />
```

This works as expected on iOS, but not on Android, where the root path is not
considered whitelisted, and therefore opens in the system browser. In order to
make it work in Android too, these two whitelist entries are needed instead of
the one above:

```xml
<allow-navigation href="https://github.com/apache/cordova-plugin-whitelist" />
<allow-navigation href="https://github.com/apache/cordova-plugin-whitelist/*" />
```

To reproduce the issue:

  1. Build the app: `cordova build`
  2. Emulate it on iOS: `cordova emulate ios`. The app should show a loading
     screen, then open the whitelist plugin GitHub page **in the app**. All is
     fine.
  3. Emulate it in Android: `cordova emulate android`. The app won't behave like
     on iOS, but instead open the GitHub page in the system browser.
