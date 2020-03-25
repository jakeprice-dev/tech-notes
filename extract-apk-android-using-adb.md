---
id: 20200325105110
uuid: 1379d4af-89d7-4947-88a4-521122fc9a95
title: Extract/Pull Android App Package from Phone
date: 2020-03-25 10:51:10
modified: 
types: tech-note
categories: android
pinned: false
tags: [android, phone, apps, apk, pull, extract, download]
private: false
draft: false
---

The `android-tools` package `sudo dnf install android-tools` on Fedora must already be installed.

List installed 3rd Party apps first.

```sh
adb shell pm list packages -f -3
```

You'll get the below output returned depending on installed 3rd Party apps.

```sh
package:/data/app/com.microsoft.office.outlook-lPqGMaQl3IKUVWIOJnjjbw==/base.apk=com.microsoft.office.outlook
package:/data/app/com.netflix.mediaclient-m77dSO43doNzrGce8c1uPA==/base.apk=com.netflix.mediaclient
package:/data/app/com.amazon.avod.thirdpartyclient-9zIJN92hmNcGK6DOSFprJQ==/base.apk=com.amazon.avod.thirdpartyclient
package:/data/app/com.disney.disneyplus-25RV0E5WzDniJQvB1srQrA==/base.apk=com.disney.disneyplus
package:/data/app/com.microsoft.teams-1CULya2XaMIuUHDYOttYwg==/base.apk=com.microsoft.teams
```

Then get the installed path of the app you want to extract.

```sh
adb shell pm path com.disney.disneyplus
```

Which will return the below.

```sh
package:/data/app/com.disney.disneyplus-25RV0E5WzDniJQvB1srQrA==/base.apk
```

Then pull the package to the local filesystem.

```sh
adb pull /data/app/com.disney.disneyplus-25RV0E5WzDniJQvB1srQrA==/base.apk ~/Media/android/apps/
```
