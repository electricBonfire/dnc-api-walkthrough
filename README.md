# DNC API

## Composer - The php package manager

### What is a package manager?

A package manager or package-management system is a collection of software tools that automates the process of installing, upgrading, configuring, and removing computer programs for a computer's operating system in a consistent manner.

#### Package Managers for other languages

* npm
* pip
* gems
* apt
* yum

## Initialize Composer

* run `composer init`

## Update `.gitignore`

```
...
/vendor/
...
```

## Installing Symfony Components

### Why Symfony

* Don't reinvent the wheel
* Great Community Support
* Components are used By Many other popular Frameworks

> ### Running Commands Inside Docker Containers
> * View running containers `docker ps`
> * To Execute a command inside a container run `docker exec  <container_name> <command> <parameters>`


To Install the http-foundation component run: `docker exec dnc-api_app_1  composer require symfony/http-foundation`

### update Dockerfile to add required dependencies

```dockerfile
FROM php:apache

RUN apt-get update -y && apt-get upgrade -y && apt install -y \
        git \
        zip \
        unzip \
        zlib1g-dev \
        libicu-dev \
        g++ \
    && docker-php-ext-configure intl \
    && docker-php-ext-install intl

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer
RUN curl -sS https://get.symfony.com/cli/installer | bash
```

### Rebuild Docker 
* `docker-compose up --build`

## PSR, Autoloading and Namespaces

* [PSR: PHP Standards Recommendations](https://www.php-fig.org/psr/)
* [PSR4 - Autoloading](https://www.php-fig.org/psr/psr-4/)

Lets look at the file: `./vendor/composer/autoload_psr4.php`

```php
<?php

// autoload_psr4.php @generated by Composer

$vendorDir = dirname(dirname(__FILE__));
$baseDir = dirname($vendorDir);

return array(
    'Symfony\\Polyfill\\Php72\\' => array($vendorDir . '/symfony/polyfill-php72'),
    'Symfony\\Polyfill\\Mbstring\\' => array($vendorDir . '/symfony/polyfill-mbstring'),
    'Symfony\\Polyfill\\Intl\\Idn\\' => array($vendorDir . '/symfony/polyfill-intl-idn'),
    'Symfony\\Component\\Mime\\' => array($vendorDir . '/symfony/mime'),
    'Symfony\\Component\\HttpFoundation\\' => array($vendorDir . '/symfony/http-foundation'),
);

```

## Lets update our index.php

```php
<?php

require_once 'vendor/autoload.php';

use Symfony\Component\HttpFoundation\Request;
use Symfony\Component\HttpFoundation\Response;

$request = Request::createFromGlobals();

$name = $request->query->get('name', "World");

return new Response(printf('Hello %s', $name));
```

`git checkout step4`
