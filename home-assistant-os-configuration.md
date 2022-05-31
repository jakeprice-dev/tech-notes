---
id: 20220531161026
uuid: 4da5326e-9557-45d2-9bea-bce6cb5c0257
title: Home Assistant OS Configuration
date: 2022-05-31 16:10:26
modified: 
types: tech-note
categories: smart-home
link: 
pinned: false
tags: [haos, home-assistant, smart-home, raspberry-pi]
private: false
draft: false
---

{{<toc>}}

I use a dedicated Raspberry Pi 4 8GB for running Home Assistant OS.

## Initial Installation

Initially, follow the installation guide here to flash the OS onto a MicroSD card for the RPi: [Raspberry Pi - Home Assistant](https://www.home-assistant.io/installation/raspberrypi)

## Post Installation Configuration

### Static IP & Hostname

#### Static IP

Assign a Static IP address in DHCP of `10.19.90.100`.

#### Hostname

Change the default hostname to the below.

```sh
ha host options --hostname "homeassistant-01"
```

Once those changes are made reboot the host.

```sh
ha host reboot
```

### Restore Backup or Create New Account

Go to http://10.19.90.100:9123 and either upload a backup, or create a new account.

### Post New Account Configuration

#### Enable Advanced Mode

Go to http://10.19.90.100:8123/profile and select the button for "Advanced Mode".

#### Enable SSH Management

Go to http://10.19.90.100:8123/hassio/addon/core_ssh/info and click "Install".

After a short wait the addon will be installed. Select the "Watchdog" button and then click "Start" to start the SSH service.

Then go to the "Configuration" tab and paste the contents of the public key into the "Authorized Keys" field, click "Save" and then in the pop-up "Restart Add-On".

Next add port 22 under the "Network" heading in the "Configuration" tab, click Save and then once-again "Restart Add-On".

Test it works by trying to ssh into the machine:

```sh
ssh root@10.19.90.100 -i ~/.ssh/homeassistant-01
```

### Install Mosquitto broker

Original instructions [here](https://mindcomponents.com/home-assistant-zigbee2mqtt-setup-with-raspbee-ii-on-raspberrypi/), but they weren't fully complete as they are below.

Go to http://10.19.90.100:8123/hassio/addon/core_mosquitto/info and click "Install".

After a short wait the addon will be installed. Select the "Watchdog" button and then click "Start" to start the service.

### Install Zigbee2MQTT

Go to http://10.19.90.100:8123/hassio/store and click the `⋮` button and then "Repositories". Paste the below into the "Add" field and then click "Add".

```
https://github.com/zigbee2mqtt/hassio-zigbee2mqtt
```

Then click "Close" and go to http://10.19.90.100:8123/hassio/addon/45df7312_zigbee2mqtt/info and click "Install".

After a short wait the addon will be installed. Select the "Watchdog" button, and "Show in sidebar" button and then click "Start" to start the service.

### Configure Zigbee2mqtt and Mosquitto

Follow the instructions here: https://log.int.ppn.sh/attachments/20220515221115.html#ssh-access--certificates

On the laptop create a key and certificate.

```sh
openssl req -newkey rsa:2048 -new \
  -nodes -x509 -days 3650 \
  -keyout ssl/mqtt_key.pem \
  -out ssl/mqtt_cert.pem
```

Then use `scp` to upload these to the `/root/ssl` directory on the Home Assistant host.

```sh
scp -i ~/.ssh/homeassistant-01 /home/jprice/ssl/* root@10.19.90.100:/root/ssl/
```

Go to http://10.19.90.100:8123/hassio/addon/core_mosquitto/config and specify the files in each field:

- Certificate File: `mqtt_cert.pem`
- Private Key File: `mqtt_key.pem`

Click Save and then "Restart Add-On".

Next go to: http://10.19.90.100:8123/config/integrations and you should now see the MQTT integration has been discovered. Click "Configure" and then in the pop-up box click "Submit" and then "Finish".

Then add the below into the "Serial" field here: http://10.19.90.100:8123/hassio/addon/45df7312_zigbee2mqtt/config

```yaml
port: /dev/serial/by-id/usb-dresden_elektronik_ingenieurtechnik_GmbH_ConBee_II_DE2490845-if00
adapter: deconz
```

Then "Save" and restart the add-on as prompted and you should not get the 502 error when accessing the webapp from the sidebar and can start adding Zigbee devices.

### Install HACS

Using SSH, run the below command to install the Home Assistant Community Store.

```sh
[core-ssh ~]$ wget -O - https://get.hacs.xyz | bash -

Connecting to get.hacs.xyz (172.67.132.174:443)
Connecting to raw.githubusercontent.com (185.199.108.133:443)
writing to stdout
-                    100% |***************************************************************************************************************************************************************|  2742  0:00:00 ETA
written to stdout
INFO: Trying to find the correct directory...
INFO: Found Home Assistant configuration directory at '/root/config'
INFO: Creating custom_components directory...
INFO: Changing to the custom_components directory...
INFO: Downloading HACS
Connecting to github.com (140.82.121.4:443)
Connecting to github.com (140.82.121.4:443)
Connecting to objects.githubusercontent.com (185.199.110.133:443)
saving to 'hacs.zip'
hacs.zip             100% |***************************************************************************************************************************************************************| 1943k  0:00:00 ETA
'hacs.zip' saved
INFO: Creating HACS directory...
INFO: Unpacking HACS...
INFO: Removing HACS zip file...
INFO: Installation complete.

INFO: Remember to restart Home Assistant before you configure it

```

Then restart Home Assistant from the system screen: http://10.19.90.100:8123/config/system

### Install HACS Add-ons

Add HACS as an Integration here: http://10.19.90.100:8123/config/integrations by clicking "Add Integration", searching for HACS, and following the setup wizard.

Go to http://10.19.90.100:8123/hacs/integrations and install the following add-ons:

- Passive BLE monitor integration
- Tapo Controller
- Tapo: Cameras Control
- WebRTC Camera

### Install Prometheus Node Exporter

Follow the instructions at [loganmarchione/hassos-addons: Home Assistant Add-ons](https://github.com/loganmarchione/hassos-addons) and then [hassos-addons/prometheus_node_exporter at main · loganmarchione/hassos-addons](https://github.com/loganmarchione/hassos-addons/tree/main/prometheus_node_exporter).

### Install Samba Backup

Follow the instructions at [hassio-addons/samba-backup at master · thomasmauerer/hassio-addons](https://github.com/thomasmauerer/hassio-addons/tree/master/samba-backup).
