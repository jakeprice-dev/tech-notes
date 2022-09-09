---
id: 20220909211339
uuid: 09381c0e-2aec-48cb-89c5-26ab448931eb
title: Clone GitHub Repository with Personal Access Token
date: 2022-09-09 21:13:39
modified: 
types: tech-note
categories: github
link: 
pinned: false
tags: [github, token, clone, personal-access-token]
private: false
draft: false
---

Create a Personal Access Token (PAT) [here](https://github.com/settings/tokens/new). Then you can use it to clone a repository, and save the need for a private key. Especially helpful on a server.

```sh
# Clone a repository:
git clone https://<token>@github.com/owner/repo.git

# Clone a branch from a repository:
git clone --branch <branch-name> https://<token>@github.com/owner/repo.git
```
