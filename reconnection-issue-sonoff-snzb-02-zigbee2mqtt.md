---
id: 20221007162437
uuid: d19e79e6-486e-4bba-8b83-304941b8d405
title: Reconnection Issue with Sonoff SNZB-02 in Zigbee2MQTT
date: 2022-10-07 16:24:37
modified: 
types: tech-note
categories: smart-home
link: 
pinned: false
tags: [sonoff, zigbee, mqtt, temperature, sensor, home-assistant, temperature, power]
private: false
draft: false
---

I had an issue with a SONOFF SNZB-02 temperature & humidity sensor, after replacing the battery I couldn't get it to reconnect to Zigbee2MQTT without the power column showing a question mark for the battery status. As a result it wasn't exposing any results. 

There was nothing in the log file either.

What eventually did the trick I think was restarting Zigbee2MQTT from Settings > Restart Zigbee2MQTT in the Zigbee2MQTT dashboard on Home Assistant. It then paired and finally published the battery status - although it doesn't seem like the battery is reporting correctly - initially it showed around 20%, and after clicking the reconfigure button it now shows around 62%, but that's still lower than expected for a new battery - unless it's been pulling power more than usual because of whatever issue was going on?
