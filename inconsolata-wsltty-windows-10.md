---
id: 20200801141931
uuid: 018d4e64-f69f-486f-918d-56e01c3b5f15
title: Inconsolata on wsltty on Windows 10
date: 2020-08-01 14:19:31
modified: 
types: tech-note
categories: windows
pinned: false
tags: [wsltty, inconsolata, fonts]
private: false
draft: false
---

When installing my favourite monospace font on Windows 10 at work It is really blurry, because when downloaded from [Inconsolata](https://www.levien.com/type/myfonts/inconsolata.html) it only seems to include `Medium` as a useable font style, and it looks horrible on wsltty.

I've found if I download from [fonts/ofl/inconsolata/static at master · google/fonts · GitHub](https://github.com/google/fonts/tree/master/ofl/inconsolata/static), specifically:

- Inconsolata Light/Medium/Regular/SemiBold 

Then in wsltty set the font to `Inconsolata`, select `Regular` for the font style, and size `13`. It's perfect, and readable again without the blurriness of medium.

I expect I could probably also just download from Google Fonts directly and accomplish the same thing.

I've placed them on my work OneDrive account for backup as well.
