# Android Security Testing Cheatsheet

## viewing logs
```
 adb shell logcat
 ```
 
 ## obtaining apps
 ### locally
 ```
 adb shell pm list packages #list packages
 adb shell pm path com.android.systemui #list path to package
 adb pull /system/priv-app/SystemUIGoogle/SystemUIGoogle.apk #download package from device
 ```
 
 ### remotely
 * https://apk-dl.com/
 * https://apps.evozi.com/apk-downloader/
 * http://apkleecher.com/
 
 ## identifying communication channels
 ### list hosts that are talked to
 ```
 sudo bettercap -T [ip-of-mobile-device] -X
 ```
 
 ### intercept http/https
 ```
 sudo bettercap -T [ip-of-mobile-device] --proxy --proxy-https --no-sslstrip
```

