# Android Phone

A rooted phone is needed. Performing the initial root is out of scope (for now). We assume the phone is rooted with Magisk v26. (Later versions might work, not tested with this guide.)

## Emulation

It is also possible to emulate the app. I've personally used [Waydroid](https://github.com/waydroid/waydroid/) and loaded Frida in it, it seems to work fairly well.

There are also [some scripts](https://github.com/casualsnek/waydroid_script) to add ARM translation if required, since Waydroid is container-based (running on a linux machine) and would run x86 Android by default.
