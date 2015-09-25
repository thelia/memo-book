#Installation

##Requirements

Thelia needs at least php 5.4 and works with php 5.5 and 5.6 for now. For the
database, Thelia requires at least MySQL 5.5.

## PHP extensions

* intl
* mcrypt
* mysql and pdo_mysql
* curl
* gd

## Download Thelia

You can download Thelia in two different ways

### From the Thelia website

Go to the thelia website (http://thelia.net) and download it.

### Using Composer

```bash
$ curl -sS https://getcomposer.org/installer | php
$ php composer.phar create-project thelia/thelia your-path 2.2.0 # replace 2.2.0 by the version of thelia you want
```

## Install Thelia

First of all, create a vhost dedicated to Thelia and put the documentRoot in
the `web` directory.

Here again you can install Thelia in two different ways

### Using install wizard

with your favorite browser, navigate to the install directory :

http://yourdomain.tld/[/subdomain_if_needed]/install

For example, I have thelia downloaded at http://thelia.net and my vhost is
correctly configured, I have to go to this address :

http://thelia.net/install

### Using cli tools

```bash
$ php Thelia thelia:install
```

and follow the instructions

**After installing Thelia, remove the web/install directory**
