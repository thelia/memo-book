# Thelia's structure

After the installation you have an architecture like this

```
www <- your web root directory
    thelia <- your thelia directory
        bin
        cache
        core
        setup
        local
            config
            modules
            session
        log
        templates
        web <- the only directory accessible by your web server
```

## Local directory

in this directory you can find four directories :

* config : contains the database.yml file. In this file is stored the
information for connecting to the database.
* modules : contains the modules developed by yourself or the community
* media : contains the media uploaded from the back-office. For example you can
 find all the product pictures.
* session : by default Thelia stores the sessions in this directory. You can
change the directory where the sessions are saved. To do this insert
a new record in the config table with ```session_config.save_path``` for the
column name and in the value column put the full path where you want to store
 the sessions.

## Template directory

The template directory contains all the templates for all Thelia's environment
: front-office, back-office, email and pdf. For each type of templates a
directory exists and contains as many template as you want but only one can be
enabled (in the back-office panel -> Configuration -> System variables)

A good practice is to duplicate the default template and then modify to have a
custom template. The default front-office template is highly
customizable, see some example at [https://github.com/thelia-templates](https://github.com/thelia-templates)

## Web directory

The web directory is the only one accessible by your web server. It contains
by default two controllers :

* index.php : used in production environement, it calculate the cache only once
and never tries to know if it is outdated or not so after each modification
**don't forget to clear the cache**
* index_dev.php : used when you develop the store. This controller is more
flexible, the cache is checked all the time and refreshed if needed. It also
logs a lot of information, like every request you make, all assets creation,
etc.

## Other directories

The other directories are less important :

* bin : propel and phpunit binaries are in this directory
* cache : the cache for each environment so you find a dev, prod and even
test directory if you run test. Each environment has is own cache so if you
clear the cache for the dev environment, only the dev environment will be
affect.
* core : Thelia's source code and third party dependencies managed with
composer.
* setup : all the files needed for bootstraping thelia : sql database, fake
catalog script, etc
* logs : Thelia's logs. The configuration is in the admin panel (configuration
-> System logs)
