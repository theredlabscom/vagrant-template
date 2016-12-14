

# Available VMs

## mysql

MySQL 5.6

    vagrant up mysql

## Apache

Apaches + PHP 5.5 + phpMyAdmin

    vagrant up apache

Web URL mapped to `public`:

    http://localhost:8043

phpMyAdmin URL:

    http://localhost:8043/pma/

## Nginx

Nginx + PHP 5.5 + phpMyAdmin

    vagrant up nginx

Web URL mapped to `public`:

    http://localhost:8044

phpMyAdmin URL:

    http://localhost:8044/pma/

## NoSQL

MongoDB + Redis

    vagrant up nosql