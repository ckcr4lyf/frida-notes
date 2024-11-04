# Android Phone

A rooted phone is needed. Performing the initial root is out of scope (for now). We assume the phone is rooted with Magisk v26. (Later versions might work, not tested with this guide.)

## Emulation

It is also possible to emulate the app. I've personally used [Waydroid](https://github.com/waydroid/waydroid/) and loaded Frida in it, it seems to work fairly well.

There are also [some scripts](https://github.com/casualsnek/waydroid_script) to add ARM translation if required, since Waydroid is container-based (running on a linux machine) and would run x86 Android by default.

### Forcing the ABI in Waydroid

Sometimes, an app may be architecture agnostic, e.g. run on both x86 and ARM. However there may be some obfuscation techniques loading a library, that is only compatible with an ARM architecture. While `libhoudini` provides a translation layer from ARM to x86, if the app is installed as x86, then it wouldn't load ARM native libraries.

However, the ABI can be forced when installing apps into Waydroid. For instance, to force an app to install "as ARM", you could oepn a shell in the device and run:

```
pm install --abi arm64-v8a [APK_PATH]
```
