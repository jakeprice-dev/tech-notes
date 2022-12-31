---
id: 20221231143123
uuid: 40831088-1fcc-467b-a85a-c27d1306d285
title: Caddy Server Basic Authentication
date: 2022-12-31 14:31:23
modified: 
types: tech-note
categories: caddy
link: 
pinned: false
tags: [caddy, authentication, basic-authentication, hash]
private: false
draft: false
---

## Basic Authentication

This will enable a prompt for a username and password when accessing the URL path. You can add additional lines for additional users.

```ini
# Template:
basicauth <path> {
    <username> <password-hash>
    <username> <password-hash>
    <username> <password-hash>
}

# Example:
basicauth / {
    jake $2a$14$piTqBgsf7igbN2FQ9fOBvusCAIYGrgSBe6sj7FaGdLQg9zJcbhzCu
}
```

You can't use a plaintext password, you must instead generate a hashed password. You can use Caddy's `hash-password` command to do this. 

Here's an example using Caddy's Docker image to hash `Password123!`.

```sh
docker run --rm -it caddy:2.6.2-alpine caddy hash-password
Enter password:
Confirm password:
$2a$14$piTqBgsf7igbN2FQ9fOBvusCAIYGrgSBe6sj7FaGdLQg9zJcbhzCu
```
