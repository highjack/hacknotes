# Android Security Testing Cheatsheet
Notes compiled of Android Security testing training, please see source at the bottom for more information.

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

## view permissions of the app
it will create a file called AndroidManifest.xml
```
apktool d file.apk
```

## reading apk java code
use jadx it's like jad but you don't need to use dex2jar first on it to see the code :)
another tool is JEB there is a free trial available it also has an arm disassmbler for any .so files that are stored in /data/data/[packagename]/lib

## making release apk debuggable
```
apktool d file.apk
```

edit AndroidManifest.xml as a property to the <application> node add 
```
 android:debuggable="true"
 ```
 now rebuild, resign the app and install
 ```
 apktool b -d application_path output.apk
 git clone https://github.com/appium/sign
java -jar sign/dist/signapk.jar sign/testkey.x509.pem sign/testkey.pk8 output.apk signed.apk
adb install signed.apk
``` 
now we should be able to debug the apk using android studio.

## instrumentation frameworks
* https://www.frida.re/
* http://repo.xposed.info/

## information sources
* https://www.evilsocket.net/2017/04/27/Android-Applications-Reversing-101/

