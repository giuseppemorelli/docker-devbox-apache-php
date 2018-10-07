# DEVBOX Apache + PHP

Dockerfile to have a development/production environment for Apache + PHP.



## How to use

You can run this command in your terminal or use `docker-compose` example file.

```bash
docker run -d --name devbox-apache-php -p 80:80 gmdotnet/devbox-apache-php:7.0 -v <your html project full path>:/var/www/html/
```



## Environment variables

| Variable name         | Description                                                  | Default Value | Example                  |
| --------------------- | ------------------------------------------------------------ | ------------- | ------------------------ |
| APACHE_USER_UID       | User ID for Apache User. Insert in most case your computer user ID | 33            | 1000                     |
| APACHE_USER_GID       | Group ID for Apache User. Insert in most case your computer group ID | 33            | 1000                     |
| POSTFIX_myhostname    |                                                              | hostname      | project.local            |
| POSTFIX_mydestination |                                                              | localhost     | project.local            |
| POSTFIX_relayhost     | SMTP Host to 'redirect' mails. Used with mailhog service for development | ''            | <mailhog-ip>:1025        |
| XDEBUG_CONFIG         | Add extra xdebug configuration.<br />In most cases we have to add the local computer ip with the same network of the docker container | ''            | remote_host=172.16.244.1 |



## What is inside

OS base system is Debian Stretch

- Apache + mod php
- All major php library
- postfix
- xdebug
- composer

In each branch you can find a complete list of the software used.



## Versions

Each branch has own version of php and kind of use (development and production)

| Version | Git Branch         | Use for     |
| ------- | ------------------ | ----------- |
| PHP 5.6 | php-5.6            | development |
| PHP 7.0 | php-7.0            | development |
| PHP 7.0 | php-7.0-production | production  |
| PHP 7.1 | php-7.1            | development |



### Notes

It's created for a quick use but it doesn't follow the main role of: you need to have 1 service for each Docker Image. 