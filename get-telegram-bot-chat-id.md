---
id: 20220811212057
uuid: 590e0154-50be-4da6-bc5b-58f19f5cf4eb
title: Get Telegram Bot Chat ID
date: 2022-08-11 21:20:57
modified: 
types: tech-note
categories: telegram
link: 
pinned: false
tags: [telegram, bot, chat-id, api]
private: false
draft: false
---

Send a message to your Telegram Bot first using the Bot token, then you can retrieve the Chat ID. 

```sh
curl --silent --request GET https://api.telegram.org/bot<bot_api_token>/getUpdates | jq '.result[]|.message.chat.id'
```

