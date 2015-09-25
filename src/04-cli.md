# CLI Tools

Thelia has a command line tool that can help you automate repetitive tasks.
Obviously you can develop your own command.

## Usage

```bash
$ cd to/thelia/repository
$ php Thelia
```

If you use the command line without any argument, it will display all command
and options available.

List of available commands :

| **command**               | **description**                                              | **example**                                                                      |
|---------------------------|--------------------------------------------------------------|----------------------------------------------------------------------------------|
| admin:create              | Create a new administrator user                              | ``` $ php Thelia admin:create```                                                 |
| admin:updatePassword      | Change administrator password                                | ``` $ php Thelia admin:updatePassword adminlogin [--pasword="..."] ```           |
| cache:clear               | Invalidate cache                                             | ``` $ php Thelia cache:clear [--env="..."] [--without-assets] [--with-images]``` |
| image-cache:clear         | Empty part or whole web space image cache                    | ```$ php Thelia  image-cache:clear [subdir]```                                   |
| generate:sql              | Generate SQL files                                           | ```$ php Thelia generate:sql [--locales="..."] ```                               |
| module:activate           | Activate a module                                            | ```$ php Thelia module:activate module-name ```                                  |
| module:deactivate         | Deactivate a module                                          | ```$ php Thelia module:deactivate module-name ```                                |
| module:generate           | Generate all needed files for creating a new Module          | ```$ php Thelia module:generate module-name ```                                  |
| module:generate:model     | Generate model for a specific module                         | ``` $ php Thelia module:generate:model module-name [--generate-sql]```           |
| module:generate:sql       | Generate the sql from schema.xml file for a specific module  | ```$ php Thelia module:generate:sql module-name```                               |
| module:list               | get module list                                              | ```$ php Thelia module:list```                                                   |
| module:refresh            | refresh module list                                          | ```$ php Thelia module:refresh```                                                |
| sale:check-activation     | check the activation and deactivation dates of sales, and perform the required action depending on the current date. | ```$ php Thelia sale:check-activation``` |
| thelia:config             | Manage (list, get, set, delete) configuration variables      | ```$ php Thelia thelia:config [--secured] [--visible] COMMAND [name] [value]```  |
| thelia:dev:reloadDB       | erase current database and create new one. **all your data will be lost** | ```$ php Thelia thelia:dev:reloadDB```                              |
| thelia:generate-resources |  Outputs admin resources                                     | ```$ php Thelia thelia:generate-resources [--output[="..."]]```                  |
| thelia:install            | Install Thelia                                               | ```$ php Thelia thelia:install```                                                |

tip: you can get help on a command using `php Thelia help COMMAND`

## Other useful commands

| **command**  | **description**  | **example** |
|---|---|---|
| ./reset_install.sh     | Reset the database, install fake data, create an administrator thelia2/thelia2 | |
| php setup/faker.php    | Install fake data (products, contents, orders, ...) | ``` $ php setup/faker.php [-c <number of categories>] [-p <number of products>] [-l <locale list>] [-r <real text>] ``` |
| php setup/demo.php     | Install the data used by the demo site of Thelia |  |
| php setup/update.php   | Update your website for a new version of Thelia |  |
