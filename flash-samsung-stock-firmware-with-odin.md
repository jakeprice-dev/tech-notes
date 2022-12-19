---
id: 20221219153033
uuid: 386a9735-3423-4ed7-a026-eab737f4ac46
title: Flash Samsung Stock Firmware with Odin
date: 2022-12-19 15:30:33
modified: 
types: tech-note
categories: android
link: 
pinned: false
tags: []
private: false
draft: false
---

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

### Download Mode

- Download and install [Samsung Android USB Driver | Samsung Developers](https://developer.samsung.com/android-usb-driver)
- Power off and boot into Download Mode by connecting it to the PC whilst holding volume up and down.
- Once in Download mode, press the volume up key.

## Flash with Odin3 v3.14 on Windows

{{<admonition warning>}}
It is crucial you use a reliable USB-C cable to avoid soft-bricking the phone. A good test is to wiggle the connection and if it disconnects when you do that, _do not use it_!
{{</admonition>}}

Begin by loading the firmware binaries in the Odin tool as instructed below:

- First, click "BL" and select the BL firmware file (BL_xxxxxxxxxxx.tar.md5)
- Click "AP" and select the AP firmware file (AP_xxxxxxxxxxx.tar.md5)
- Next, click "CP" and select the CP firmware file (CP_xxxxxxxxxxx.tar.md5)
- Finally, click "CSC" and select the CSC firmware file (CSC_xxxxxxxxxxx.tar.md5)

Once all the firmware files have been loaded in their corresponding slots, click the "Options" tab in Odin and uncheck "Auto Reboot" (This is important). Finally, click "Start" to begin the flashing process.

After the flashing process is complete, you should see a "PASS!" message in Odin and your Galaxy S20 can be rebooted from Download mode. 

You can close Odin and disconnect your phone from the computer.

After a while you should see the "SAMSUNG" boot logo, and the ROM should boot.

{{<admonition >}}
On Android 13, to stop the intermittant "Access denied" pop-up, disable "Samsung Wallet Digital Key" from being allowed to "Appear on top".
{{</admonition>}}

