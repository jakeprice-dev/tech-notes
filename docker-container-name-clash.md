---
id: 20220601163801
uuid: b25b2def-296e-45db-9135-3a65ee04063e
title: Regular File Not Found Error PHP Container
date: 2022-06-01 16:38:01
modified: 
types: tech-note
categories: docker
link: https://stackoverflow.com/a/55040196/9625225
pinned: false
tags: [php, docker, caddy]
private: false
draft: false
---

I was having an issue launching another PHP application on the same device with Docker Compose. 

> You are having this issue because you have more than one container with same hostname (php) in same network.

{{<embed_html file="/attachments/20220601163801.html">}}

As the above makes clear, it was because I had copied the `compose.yml` and used the same service name for the PHP container in Compose and the Caddyfile. Changing `php` to `<app-name>-php` in both Compose and Caddyfile fixed it.

#### Photos

##### Compose

```yaml
  php:
    image: php-with-extensions:7.4
    container_name: photos-01-php-fpm
```

##### Caddyfile

```
:8091 {
    root * /media/photos
    file_server
    php_fastcgi php:9000
}
```

#### List

##### Compose

```yaml
  php:
    image: php-with-extensions:7.4
    container_name: list-01-php-fpm
```

##### Caddyfile

```
:8098 {
    root * /var/www/html
    file_server
    php_fastcgi php:9000
}
```
