<h1 align="center">Docker Apache + PHP</h1>

![PHP 5.6](https://github.com/giuseppemorelli/docker-devbox-apache-php/actions/workflows/php-5.6-debian.yml/badge.svg)
![PHP 7.0](https://github.com/giuseppemorelli/docker-devbox-apache-php/actions/workflows/php-7.0-debian.yml/badge.svg)
![PHP 7.1](https://github.com/giuseppemorelli/docker-devbox-apache-php/actions/workflows/php-7.1-debian.yml/badge.svg)
![PHP 7.2](https://github.com/giuseppemorelli/docker-devbox-apache-php/actions/workflows/php-7.2-debian.yml/badge.svg)
![PHP 7.3](https://github.com/giuseppemorelli/docker-devbox-apache-php/actions/workflows/php-7.3-debian.yml/badge.svg)
![PHP 7.4](https://github.com/giuseppemorelli/docker-devbox-apache-php/actions/workflows/php-7.4-debian.yml/badge.svg)
![PHP 8.0](https://github.com/giuseppemorelli/docker-devbox-apache-php/actions/workflows/php-8.0-debian.yml/badge.svg)
![PHP 8.1](https://github.com/giuseppemorelli/docker-devbox-apache-php/actions/workflows/php-8.1-debian.yml/badge.svg)


Dockerfile to have a development environment for Apache + PHP.

## How to use

You can run this command in your terminal or use `docker-compose` example file.

```bash
docker run -d --name devbox-apache-php -p 80:80 giuseppemorelli/devbox-apache-php:7.4 -v <your html project full path>:/var/www/html/
```

## Environment variables

| Variable name         | Description                                                                                                                           | Default Value | Example                  |
| --------------------- |---------------------------------------------------------------------------------------------------------------------------------------| ------------- | ------------------------ |
| APACHE_USER_UID       | User ID for Apache User. Insert in most case your computer user ID                                                                    | 33            | 1000                     |
| APACHE_USER_GID       | Group ID for Apache User. Insert in most case your computer group ID                                                                  | 33            | 1000                     |
| POSTFIX_myhostname    |                                                                                                                                       | hostname      | project.local            |
| POSTFIX_mydestination |                                                                                                                                       | localhost     | project.local            |
| POSTFIX_relayhost     | SMTP Host to 'redirect' mails. Used with mailhog service for development                                                              | ''            | <mailhog-ip>:1025        |
| XDEBUG_CONFIG         | Add extra xdebug configuration.<br />In most cases we have to add the local computer ip with the same network of the docker container | ''            | remote_host=172.16.244.1 |

See [docker-devbox](https://github.com/giuseppemorelli/docker-devbox) for more info

## What is inside

OS base system is Debian Stretch

- Apache + mod php
- All major php library
- postfix
- xdebug
- composer 2

In each branch you can find a complete list of the software used.



## Versions

Each subfolder has own version. Main tag is: `giuseppemorelli/devbox-apache-php`, then you can choose the tag version from this list

| Version | Subfolder      | Docker Hub tag                         |
|---------|----------------|----------------------------------------|
| PHP 5.6 | php-5.6-debian | giuseppemorelli/devbox-apache-php:5.6  |
| PHP 7.0 | php-7.0-debian | giuseppemorelli/devbox-apache-php:7.0  |
| PHP 7.1 | php-7.1-debian | giuseppemorelli/devbox-apache-php:7.1  |
| PHP 7.2 | php-7.2-debian | giuseppemorelli/devbox-apache-php:7.2  |
| PHP 7.3 | php-7.3-debian | giuseppemorelli/devbox-apache-php:7.3  |
| PHP 7.4 | php-7.4-debian | giuseppemorelli/devbox-apache-php:7.4  |
| PHP 8.0 | php-8.0-debian | giuseppemorelli/devbox-apache-php:8.0  |
| PHP 8.1 | php-8.1-debian | giuseppemorelli/devbox-apache-php:8.1 |

or check directly on [Docker Hub](https://hub.docker.com/repository/docker/giuseppemorelli/devbox-apache-php/tags?page=1&ordering=name)

### Notes

It's created for a quick use but it doesn't follow the main role of: you need to have 1 service for each Docker Image. 
