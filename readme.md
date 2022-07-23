**Readme**
-
**Nginx + PHP-FPM + Opcache + Extension**

**PHP PDO Extension** 
-
<u>**Base on :: php:${latest}-fpm-bullseye**</u>
- mysql
- sqlsrv version 5.10.0
- ldap
- pgsql

**NGINX Config**
-
- base from [NGINX](https://www.digitalocean.com/community/tools/nginx)

**PHP Config**
-
- memory_limit = 512M
- max_execution_time = 600

**PHP-FPM Config**
-
- request_terminate_timeout = 900

**OP Cache**
-

- opcache.enable=1
- opcache.memory_consumption=512
- opcache.interned_strings_buffer=64
- opcache.max_accelerated_files=524521
- opcache.max_wasted_percentage=15
- opcache.save_comments=1
- opcache.mmap_base=0x20000000
