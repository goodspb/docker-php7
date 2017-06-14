# docker-php7 - Docker tooling for php developer


### Table of Contents

* [Description](#description)
* [Usage](#usage)

Description
===========

docker-compose include services below:

* nginx 1.12.0
* php 7.1.5 (with-ext: redis、phalcon 3.1.2、swoole 2.0.8、curl, json, mcrypt, openssl, PDO, PDO-mysql, mbstring)
* mysql 5.7
* redis 3.2.9

Usage
=====

Make sure that `docker-compose` is installed.

> start docker

```bash
docker-compose up -d
```

> stop docker

```bash
docker-compose stop
```

> restart docker

```bash
docker-compose restart
```

> bash

```bash
docker-compose exec nginx bash
docker-compose exec php bash
```

> nginx config directory

```bash
./services/nginx/conf.d     # docker-composer should restart after adding new config 
```

> location of sites

```bash
wwwroot
```
