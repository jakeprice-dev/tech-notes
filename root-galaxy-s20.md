---
id: 20211115210143
uuid: ab39457a-10a9-4ebb-96c2-78209deae7da
title: Root Samsung Galaxy S20
date: 2021-11-15 21:01:44
modified: 
types: tech-note
categories: android
pinned: false
tags: [samsung, galaxy-s20, android, root]
private: false
draft: false
---

## Helpful Guides

There's a lot of absolute rubbish out there when it comes to Android guides for things like rooting, but the below from "The Custom Droid" are very good and have always worked for me.

- [Root Galaxy S20, S20+, and S20 Ultra using Magisk (Guide)](https://www.thecustomdroid.com/samsung-galaxy-s20-magisk-rooting-guide/) - [Local Archive](/attachments/20220211083814-samsung-galaxy-s20-magisk-rooting-guide.html)
- [How to Relock the Bootloader on Samsung Galaxy S10/S10+/S10e](https://www.thecustomdroid.com/relock-samsung-galaxy-s10-bootloader-guide/) - [Local Archive](/attachments/20220211143716-relock-samsung-galaxy-s10-bootloader-guide.html)
- [Download and Install Stable One UI 3.0 Update on Galaxy S20 Series](https://www.thecustomdroid.com/galaxy-s20-series-one-ui-3-installation-guide/) - [Local Archive](/attachments/20220211144348-galaxy-s20-series-one-ui-3-installation-guide.html)

And also the below from Magisk.

- [Installation | Magisk](https://topjohnwu.github.io/Magisk/install.html#samsung-system-as-root) - [Local Archive](/attachments/20220211144549-install.html)

{{<admonition warning>}}
Unlocking the bootloader will erase all data on the phone.
{{</admonition>}}

- Unlock Bootloader. Developer options > OEM Unlocking > True
- Power off and boot into Download Mode by connecting it to the PC whilst holding volume up and down.
- In Download Mode, long press volume up to enter device unlock mode, and then press volume up to confirm yes.
- The phone will now restart and wipe all data, which may take a few minutes, before booting back into the Android initial setup screen.
- Once booted go through the initial setup again.
- Re-enable Developer options by going to Settings > About phone > Software information > Pressing Build number 7 times.
- Go to Developer options and make sure OEM unlocking is toggled on and greyed out.

## Download Official Stock Firmware

Use [samloader](https://github.com/nlscc/samloader) to download official stock firmware from Samsung's servers. You can also look at [SAMMOBILE](https://www.sammobile.com/samsung/galaxy-s20/firmware/SM-G980F/BTU/#SM-G980F) to see the latest for this device.

### Install samloader

```sh
pip3 install --user git+https://github.com/nlscc/samloader.git
```

### Download Firmware

```sh
# Create download directory:
mkdir ~/Downloads/samsung

# Check the lastest available firmware:
samloader --dev-model SM-G980F --dev-region BTU checkupdate

# Download the specified firmware version:
samloader --dev-model SM-G980F --dev-region BTU download --fw-ver G980FXXUDEVA9/G980FOXMDEVA9/G980FXXUDEVA9/G980FXXUDEVA9 --out-dir ~/Downloads/samsung

# Decrypt firmware:
samloader --dev-model SM-G980F --dev-region BTU decrypt --fw-ver G980FXXUDEVA9/G980FOXMDEVA9/G980FXXUDEVA9/G980FXXUDEVA9 --enc-ver 4 --in-file ~/Downloads/samsung/SM-G980F_3_20220126141002_sqgfarrj77_fac.zip.enc4 --out-file ~/Downloads/samsung/SM-G980F_3_20220126141002_sqgfarrj77_fac.zip
```

### Extract Firmware

{{<admonition note>}}
If initial extraction fails, download the firmware again.
{{</admonition>}}

```sh
unzip ~/Downloads/samsung/SM-G980F_3_20220126141002_sqgfarrj77_fac.zip -d ~/Downloads/samsung/
```

- Copy only the AP file to the phone `AP_G980FXXUDEVA9_G980FXXUDEVA9_MCL23524635_MQB48658115_REV01_user_low_ship_MULTI_CERT_meta_OS12.tar.md5` and paste it into `Download`. It'll take a few minutes to transfer.

```sh
adb push AP_G980FXXSFGVK7_G980FXXSFGVK7_MQB59365521_REV01_user_low_ship_MULTI_CERT_meta_OS13.tar.md5 /sdcard/Download
```

- Download and copy the latest version of the Magisk APK from [GitHub](https://github.com/topjohnwu/Magisk/releases/) to the phone.
- On the phone install the Magisk APK and open it.
- Within the app, in the Magisk grey box click `Install` and then `Select and Patch a File`, then select the AP file you just copied to the phone.
- Click `LET'S GO >` and Magisk will download the latest version of of Magisk and patch the AP firmware with it. The process can take a few minutes.
- When the log says `- All done!` you can click the back button and in the `Download` directory you will now see a new file `magisk_patched-24100_iecfW.tar`.
- Copy this new file back to the PC.
- Power off and boot into Download Mode by connecting it to the PC whilst holding volume up and down.
- Once in Download mode, press the volume up key.

## Flash with Odin3 v3.14 on Windows

{{<admonition warning>}}
It is crucial you use a reliable USB-C cable to avoid soft-bricking the phone. A good test is to wiggle the connection and if it disconnects when you do that, _do not use it_!
{{</admonition>}}

Begin by loading the firmware binaries in the Odin tool as instructed below:

- First, click "BL" and select the BL firmware file (BL_xxxxxxxxxxx.tar.md5)
- Click "AP" and select the Magisk-patched AP firmware file (magisk_patched.tar)
- Next, click "CP" and select the CP firmware file (CP_xxxxxxxxxxx.tar.md5)
- Finally, click "CSC" and select the CSC firmware file (CSC_xxxxxxxxxxx.tar.md5)

Once all the firmware files have been loaded in their corresponding slots, click the "Options" tab in Odin and uncheck "Auto Reboot" (This is important). Finally, click "Start" to begin the flashing process.

After the flashing process is complete, you should see a "PASS!" message in Odin and your Galaxy S20 should reboot automatically. You can close the tool and disconnect your phone from the computer.

### Root the Phone

- Run through the intial setup again, and then launch the Magisk app from the app list and when prompted click `OK` to `Upgrade to full Magisk to finish the setup. Download and install?` then Magisk will be installed.
- Open the full version of Magisk and you'll be prompted for additional setup, so you can click OK. The phone will then reboot.
- That's it the phone is now rooted.

## Un-Root Phone

To un-root, click the `Uninstall Magisk` button in the Magisk app, then click `Complete Uninstall`. Magisk will then uninstall and reboot the device and you will no longer be rooted.

This didn't work, so it's easier to just reflash from scratch.
