#Installation

##Requirements

Thelia needs at least php 5.4 and works with php 5.5 and 5.6 for now. For the
database, Thelia requires at least mysql 5.5.

## PHP extensions

* intl
* mcrypt
* mysql and pdo_mysql
* curl
* gd

## Download and install Thelia

```bash
$ curl -sS https://getcomposer.org/installer | php
$ php composer.phar create-project thelia/thelia your-path 2.0.3
$ php Thelia thelia:install
```

**After installing Thelia, remove the web/install directory**



