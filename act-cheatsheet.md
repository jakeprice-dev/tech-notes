---
id: 20220808110514
uuid: 47e6d68e-00cc-4167-9b67-3a40df279584
title: act Cheatsheet
date: 2022-08-08 11:05:14
modified: 
types: tech-note
categories: github
link: https://github.com/nektos/act
pinned: false
tags: [github, action, runner, local, ci-cd, pipeline]
private: false
draft: false
---

```sh
# Run act with specific workflow, include a secrets file and specify container architecture:
act --workflows .github/workflows/azure_app_service.yml --secret-file .secrets --container-architecture linux/amd64

# Run act with a place to put an artifact:
act --workflows .github/workflows/azure_app_service.yml --secret-file .secrets --container-architecture linux/amd64 --artifact-server-path /tmp/act-artifacts
```