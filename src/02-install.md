#Installation

##Requirements

Thelia needs at least php 5.4 and works on php 5.5 and 5.6 for now. For the
database, Thelia requires at least mysql 5.5.

## PHP extensions

* intl
* mcrypt
* mysql and pdo_mysql
* curl
* gd

## Download Thelia

You can download Thelia by two different way

### From Thelia website

Go to thelia website (http://thelia.net) and download it.

### Using Composer

```
curl -sS https://getcomposer.org/installer | php
php composer.phar create-project thelia/thelia your-path 2.0.3
```

## Install it

First of all, create a vhost dedicated to Thelia and put the documentRoot in
the web directory.

Here again you can install Thelia by two different way

### Using install wizard

with your favorite browser, navigate to the install directory :

http://yourdomain.tld/[/subdomain_if_needed]/install

For example, I have thelia downloaded at http://thelia.net and my vhost is
correctly configured, I have to reach this address :

http://thelia.net/install

### Using cli tools

```
php Thelia thelia:install
```

and follow the instructions

**After installing Thelia, remove the web/install directory**



