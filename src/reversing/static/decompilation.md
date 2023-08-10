# Decompilation

Android apps can be decompiled to Java source fairly well, this can help significantly in understanding what's going on. 

One of the best tools for this is [jadx](https://github.com/skylot/jadx). Note: JADX is quite memory 

## Pull the Application

We need to identify the application on the Android device and pull the associated APK. This can also be done via apps such as [Split APKs Installer](https://play.google.com/store/apps/details?id=com.aefyr.sai).

```
$ adb shell ps -A | grep leavehomesafe
u0_a248       17599  17769 23390396 225972 0                  0 S hk.gov.ogcio.leavehomesafe

$ adb shell pm path hk.gov.ogcio.leavehomesafe
package:/data/app/~~aHXXxw9ISu-FhyhkqxOpsw==/hk.gov.ogcio.leavehomesafe-2uNVfPN1SDS4vCfz42WFlw==/base.apk

$ adb pull /data/app/~~aHXXxw9ISu-FhyhkqxOpsw==/hk.gov.ogcio.leavehomesafe-2uNVfPN1SDS4vCfz42WFlw==/base.apk
/data/app/~~aHXXxw9ISu-FhyhkqxOpsw==/hk.gov.ogcio.leavehomesafe-2uNVfPN1SDS4vCfz42WFlw==/base.apk: 1 file pulled, 0 skipped. 37.8 MB/s (58567607 bytes in 1.478s)
```

Now we can launch jadx on it:

```
jadx-gui base.apk
```

This can help determine stuff such as which different classes are being used, what is being passed around, who is calling whom etc. 

A lot of this would be useful when we are dynamically changing stuff with Frida.

### References

* https://doubleagent.net/2023/05/21/a-car-battery-monitor-tracking-your-location