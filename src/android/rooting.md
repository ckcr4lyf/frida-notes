# Rooting

## Recommended Phones

I'd personally recommed a Pixel for rooting. They come with true stock Android and almost no restrictions. [Additionally Google provides very convenient official download links to the firmware for all models and versions](https://developers.google.com/android/images), which makes the process quite straightforward.

For this guide, I'm going to proceed with a Pixel 6a, which I picked up from Taobao (2nd hand) for ~800RMB.

## Rooting the Phone

Warning: This process will completely wipe all data from the phone!

### Prepare the phone

Enable developer options in your phone, and then go into it. Make sure OEM unlocking is enabled.

On your laptop, pair the device with `adb` once as well. (e.g. just run `adb shell` and accept the connection).

Finally, [download MagiskManager](https://github.com/topjohnwu/Magisk/releases) on your phone, this will be used to patch the firmware.

### Patching the firmware

First, we need to download the official firmware from the [Google download page](https://developers.google.com/android/images#bluejay).

This will download to a file like `bluejay-ap2a.240605.024-factory-6fcf1c35.zip`. Extract this archive, and inside will be another zip file. For me it was `image-bluejay-ap2a.240605.024.zip`. Extract this guy as well.

Now, you should have a file called `boot.img`. Transfer this to your Android device. The easiest way is to use adb to push it to your phone:

```
adb push boot.img /sdcard/Download
```

Then, open the Magisk app. Select "Install", and then "Patch". Select your `boot.img` file, and let Magisk patch it. This should produce a file like `magisk_patched-27000_xvyfB.img`. Transfer this back to your computer:

```
adb pull /sdcard/Download/magisk_patched-27000_xvyfB.img .
```

### Flashing the patched firmware

The next few commands we run will reboot the phone to the booatloader, unlock it for flashing, and then flash the patched image. 

We'll first wipe the phone:

```
adb reboot bootloader
fastboot erase userdata
fastboot reboot
```

Now that it's reset, you'll need to enable developer options and USB debugging again. Then:

```
adb reboot bootloader
fastboot flashing unlock
fastboot flash boot magisk_patched-27000_xvyfB.img
fastboot reboot
```

That's it! You're technically now root! Next we'll install Magisk and some Modules...

## Disable Updates

Before we even connect the device to the internet, it's a good idea to disable all updates. The reason is sometimes if something like PlayServices get's updated, then the fixes around PlayIntegrity (formerly SafetyNet) we do will stop working.

1. Open Play Store. It will ask you to Sign In, but in the top right you'll have three dots, from where you can uncheck "Auto-Update Google Apps"
2. In developer options, navigate down to "Automatic system updates", and disable this

## Magisk and other Modules

First, we will download some applications:

* [YASNAC](https://github.com/RikkaW/YASNAC) - Used to verify if we pass SafetyNet
* [spic-andriod](https://github.com/herzhenr/spic-android) - Used to verify if we pass PlayIntegrity

Download them and run both tests, they should fail (this is ok). We will now fix it.

Open the existing Magisk app once. It'll ask for an update, and then when you open it again it'll reboot the device. Then, in Magisk settings, enable "Zygisk" and "Enforce Denylist", and reboot once more.

Now, we will download the [PlayIntegrityFix](https://github.com/chiteroman/PlayIntegrityFix/releases) module. This will help us pass the integrity checks used by some apps.

Download the ZIP file, and then in Magisk, open the modules section. Install this module, and then reboot.

Now try running YASNAC and spic-android again. YASNAC should pass both checks, and spic should show `DEVICE_INTEGRITY`. This should be sufficient for most applications. Anyway for the tricky ones, we can patch dynamically at runtime with Frida!

