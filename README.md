# TWRP recovery for UHANS S1

### Summary

This is an unofficial port of the Team Win Recovery Project's v3.0.2-1 release to the Chinese UHANS S1 Android device.

## TL;DR
Just get me the latest build:

[![Latest Release](https://img.shields.io/badge/TWRP-v3.0.2--1-blue.svg)](https://github.com/almasen/twrp_android_device_uhans_s1/releases/latest)

Note: these builds do and will NOT contain a scatter file. Please refer to the [flashing instructions](README.md#Instructions) if you are unsure how to proceed.

## Introduction

The main purpose of the repository is to publicly release a version of the TWRP open source recovery that is suitable for and functional on the UHANS S1 model and to provide the source of the built recovery image, to allow existing users to gain greater control of their handsets, therefore, among others, allowing them to modify the official Freeme OS build running on their device and gain root privileges.

### Importance

As to our understanding there does not exist a safe and functional way of rooting the S1 device that does not involve the installation of a custom recovery image. This TWRP recovery is essential to those users affected by the malicious system applications present on some of the S1 models and would wish to remove those from their devices.

### Malware

Many UHANS handsets running Freeme OS v2.39+ out of the box came preinstalled with a [Trojan](https://www.virustotal.com/gui/file/0dc1f7f8bf346c88890bbfdf3c577171a86aa97a74e7d638a5417439b734c4e9/) disguised as the Official YouTube application. The fake YouTube application was functional, detached from the Play Store and had high level device permissions, was installed as a priv-app, and ultimately had access to information about the user's Google Account. The application has been detected as malicious by the Google Play Store since March 2018, therefore also warning users unaware of the fake video player. The YouTube apk could be disabled via the settings app, the Play Store's prompt, or third party tools, however, could also re-enable itself after a reboot, and can only be removed with root privileges. The European sales of the devices got terminated after the discovery of the malicious intent, and the company went out of business in 2019 allowing the release of certain previously confidential device sources excluding kernel sources. The other releases, such as the stock ramdisk can be found on my profile.

### License

As per our agreement with my former superior at the European Development Department of UHANS Mobile affiliated with UHANS Mobile Inc, from the 1st August 2019, this recovery is released publicly to the open source community licensed under GPL v3. Therefore, you are very welcome to, first and foremost, freely use it, as well as, modify and rebuild this recovery to your liking and share modified versions of it publicly.

### Credits

Many thanks to the developers of the open source Team Win Recovery Project for making this possible. Please kindly consider supporting them for their precious hard work: https://twrp.me/

## Instructions

### Disclaimer

Team Win and I take no responsibility for any damage that may occur from installing or using TWRP on your device.

#### No scatter

Since this repository's primary aim is to provide users a way of gaining greater control over their devices, most importantly to those users affected by malicious system applications, my aim is to provide the simplest solution possible, as to most average users, flashing a built recovery image is already complex enough of a daily task.

Contrary to common belief this device does not require a pre-written scatter file in order to flash a custom recovery image, and due to its simplicity when flashing via the Fastboot interface, I decided not to include a scatter.txt or scatter writing instruction in our builds. Moreover, this way users don't have to use third party tools, such as the SP Flash Tool, which is not only confusing, but is also often distributed over the internet with bundled malware. Aiming simplicity is also the reason that there are no build instructions here, but as per the license, you are obviously welcome to use the source to your liking and to use different flashing mechanisms.

### Instructions to flashaholics

Simply flash the recovery via the bootloader and reboot into it to secure the partition.
If it is still overridden by the next boot, flash the image again in TWRP.


### Step-by-step for those new to flashing

Is your bootloader unlocked? If yes, feel free to skip to step [#2](README.md#2-Flashing) Otherwise, just follow along:

#### 1. Unlocking

This process will unlock the bootloader of your device allowing modifications to it's operating system partitions.

  **Warning:** this process will wipe the information on your smartphone similarly to a factory reset, so make sure to create a full backup of your data before starting the process.

**1.0 Prerequisites**

Make sure the Android SDK Command Line Platform Tools, hence the adb and fastboot interfaces and drivers are properly installed on your system. There are countless guides and tools on the internet to help you with this, please start with the [official documentation](https://developer.android.com/studio/releases/platform-tools) if you are new to this topic.
Make sure to double check that adb and fastboot commands are properly working on your system before proceeding to the next steps. You can easily check this by typing 'adb' and 'fastboot' in a terminal.

**1.1 Enable developer options**

Go to Settings/About phone and tap build number 7 times quickly. It should prompt you `You are now a developer` :blush:

**1.2 Enable OEM unlocking**

Go to Settings/Developer options (this should show up after the previous step) and enable `OEM unlocking`. You may get a warning that device protection features will no longer work, you may disregard this warning, or read up on it [here](https://www.androidpolice.com/2015/03/12/guide-what-is-android-5-1s-antitheft-device-protection-feature-and-how-do-i-use-it/).
Make sure to double check that the flag is turned on before proceeding to the next step.

**1.3 Reboot to bootloader**

1.3.1 Turn off your device

1.3.2 Press and hold the `Volume Up` + `Power` keys until a little textual menu appears

1.3.3 Navigate to the `Fastboot Mode` option with the `Volume Up` key (keep pressing it until the right item is selected), then enter `Fastboot Mode` by pressing the `Volume Down` key.

1.3.4 You should now see a little text stating that your device is in `Fastboot mode`. If this did not come up, or a different window appears, don't worry, you should be able to restart your device by long pressing the power button, and you should re-attempt the steps at 1.3.

1.3.5 Plug your device into your computer with a Micro USB cable. Depending on your operating system a sound and/or prompt may notify you upon detection of the device. To make sure your phone has been detected, open a terminal window and type the `fastboot devices` command. If you correctly installed the required drivers executing this command should show your device's serial number as well as the fastboot text in line with it:
```
PS C:\Users\UHANSd> fastboot devices
EEZPBMNZ9LKLKAHE        fastboot
```
If the command executed without any result, you should reattempt the installation of the fastboot drivers.

**1.4 OEM Unlock**

Now we are ready to unlock the bootloader of the device. You should check the status of your device by typing ```fastboot getvar unlocked``` which should say:
```
PS C:\Users\UHANSd> fastboot getvar unlocked
unlocked: no
```
If it says otherwise, there is a good chance, your device already has an unlocked bootloader. In this case you should attempt steps from [#2](README.md#2-Flashing) to check if you are able to flash a custom recovery.

If it is confirmed that your device is not unlocked you are ready to start the unlocking process.

**Warning:** this will wipe all data from your device, so make sure you have backed up everything, including files on your internal storage, sms messages, important application data, etc.

To unlock, type the following command: ```fastboot oem unlock```

This should result in your terminal awaiting a response from your device, and on the device you should see a new prompt asking you to confirm the unlock. Please read the information carefully and proceed with the unlocking via pressing the appropriate volume key.

Upon a successful unlock your terminal should confirm that the process has finished and your device may reboot. If your device does not reboot automatically you can reboot it by typing ```fastboot reboot```.

This reboot may take longer than usual and it will likely ask you to set up your operating system similarly to a factory reset.

#### 2. Flashing

This process will install, in order words, flash, the desired TWRP Recovery image onto your device.

**2.0 Prerequisites**

- Make sure that `OEM Unlocking` is (still) enabled in Settings/Developer options. You may need to re-enable Developer options to view this.
- Reboot to Fastboot and connect your device to your computer (just like before)
- Make sure that your device is successfully unlocked by typing ```fastboot getvar unlocked```
- Download the latest recovery image from the [Releases](https://github.com/almasen/twrp_android_device_uhans_s1/releases/latest) section of this repository if you haven't yet

**2.1 Flashing the recovery**

2.1.1 Navigate to the directory where the downloaded recovery image is stored and open a terminal window from this folder. If you need help with this step, just do a quick google search quoting your operating system.

If the recovery image is in your Downloads folder, the path should look similar to this:

```
PS C:\Users\UHANSd\Downloads>
```

2.1.2 Type ```fastboot devices``` to make sure your device is recognised

2.1.3 Flash the recovery by executing the following command:  ```fastboot flash recovery twrp-3.0.2-1-uhans-S1.img```

The flashing process should look something like this:
```
PS C:\Users\UHANSd\Downloads> fastboot flash recovery twrp-3.0.2-1-uhans-S1.img
target reported max download size of 134217728 bytes
sending 'recovery' (11788 KB)...
OKAY [  0.950s]
writing 'recovery'...
OKAY [  0.281s]
finished. total time: 1.247s
```

**2.2 Reboot to recovery**

Now reboot to the recovery by first typing the ```fastboot reboot``` command, then *quickly pressing* the `Volume Up` + `Power` keys. You should see the familiar boot menu show up, from which, you should select the `Recovery Mode` option this time.

2.3 On a successful installation you should see the TWRP logo show up and a prompt related to unlocking the device.

2.4 Enjoy your powerful recovery

## Help

Feel free to drop me a message should you require help or have any questions regarding this project.
