---
id: 20220718202153
uuid: d0a0feed-e760-447a-b21d-52cbf33e610e
title: Sky Fibre Setup with Netgear R7800 Router
date: 2022-07-18 20:21:53
modified: 
types: tech-note
categories: network
link: https://helpforum.sky.com/t5/Broadband/PPPOE-codes-for-connection-to-third-party-router/m-p/3899878#M284210
pinned: false
tags: [sky, fibre, ont, broadband, openreach, fttp, client-identifier-string, option-61]
private: false
draft: false
---

Setup is _really_ easy. No need to use the Sky Router at all. 

Under `Advanced Setup` on the R7800 [Administration Page](http://10.19.90.1/adv_index.htm) go to `Setup` > `Internet Setup`, and under `DHCP Option` you should see a field for `Client Identifier String (Option 61)`. Paste the following string into the text field:

```
anything@skydsl|anything
```

You can replace the `anything` but you actually don't need to. Once you click `Apply`, you should immediately pick up an IP Address in the `Internet IP Address` section, as long as `Get Dynamically from ISP` is selected.

Screenshot of the settings below.

![](/attachments/20220718202153_1.png)

Screenshot of the comment below and [here](https://helpforum.sky.com/t5/Broadband/PPPOE-codes-for-connection-to-third-party-router/m-p/3899878#M284210) at Sky's community forum.

![](/attachments/20220718202153_2.png)
