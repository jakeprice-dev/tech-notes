---
id: 20221118144822
uuid: 0d14c4c9-fb77-4a5e-a3a1-f50e39bfc37f
title: macOS cron Permission Issues
date: 2022-11-18 14:48:22
modified: 
types: tech-note
categories: macos
link: 
pinned: false
tags: [cron, macos, permission, hugo]
private: false
draft: false
---

I use macOS at work, and I have a number of cron jobs that I set to run. 

One of the jobs run's on a regular basis, and is supposed to delete the `public/` directory of a Hugo static site, and then rebuild the site. Trouble is it always fails to remove the directory because it doesn't have permissions to do it.

```sh
rm: /Users/jprice/drive/my/files/documents/log/public: Operation not permitted
```

It's a "simple" enough fix. To get around it you have to give `/usr/sbin/cron` _Full Disk Access_ in macOS. 

{{<admonition tip>}}
You'll need to run `Cmd-Shift-G` to bring up a "Go-To" box to get to the `/usr/sbin` directory.
{{</admonition>}}

To do that go to "System Preferences > Security & Privacy > Privacy > Full Disk Access" or follow, [these instructions](https://osxdaily.com/2020/04/27/fix-cron-permissions-macos-full-disk-access/) - there's no command you can run ☹️. 
