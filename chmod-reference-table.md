---
id: 20190113163901
uuid: 6335c00d-5cbb-4f09-9d12-644a92d308f8
title: chmod Reference Table
date: 2019-01-13 16:39:01
modified: 
types: tech-note
categories: linux
link: https://dbiers.me/chmod-permissions-reference-chart/
pinned: false
tags: [chmod, linux, permissions]
private: false
draft: false
---

A helpful overview of `chmod` permissions.

| Permission | User | Group | Other |
|------------|------|-------|-------|
| Read       | 4    | 4     | 4     |
| Write      | 2    | 2     | 2     |
| Execute    | 1    | 1     | 1     |
|            | U    | G     | O     |
|            | X    | X     | X     |

| Numeric | Permission |
|---------|------------|
| 777     | rwxrwxrwx  |
| 755     | rwxr-xr-x  |
| 644     | rw-r–r–    |
| 700     | rwx——      |
| 750     | rwxr-x—    |
