---
id: 20220208131324
uuid: 26caed21-38f4-4ead-9645-4f3330334870
title: Code Server Port Bind Issue
date: 2022-02-08 13:13:24
modified: 
types: tech-note
categories: self-hosted
link: 
pinned: false
tags: [code-server, port, bind, mount, docker]
private: false
draft: false
---

If you want to change the port [Code Server](https://github.com/coder/code-server) uses, it's hard-coded into the `/etc/services.d/code-server/run` script, as a value for the `--bind-addr` flag.

```sh
#!/usr/bin/with-contenv bash

if [ -n "${PASSWORD}" ] || [ -n "${HASHED_PASSWORD}" ]; then
    AUTH="password"
else
    AUTH="none"
    echo "starting with no password"
fi

if [ -z ${PROXY_DOMAIN+x} ]; then
    PROXY_DOMAIN_ARG=""
else
    PROXY_DOMAIN_ARG="--proxy-domain=${PROXY_DOMAIN}"
fi

exec \
    s6-setuidgid abc \
        /app/code-server/bin/code-server \
            --bind-addr 0.0.0.0:8088 \
            --user-data-dir /config/data \
            --extensions-dir /config/extensions \
            --disable-telemetry \
            --auth "${AUTH}" \
            "${PROXY_DOMAIN_ARG}" \
            "${DEFAULT_WORKSPACE:-/config/workspace}"
```

I mount the file in Docker Compose so I can override `--bind-addr`:

```yml
    volumes:
      - ./run:/etc/services.d/code-server/run
```
