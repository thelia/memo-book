# CLI Tools

Thelia has a command line tools that can help you automate repetitive tasks.
Obviously you can develop your own command.

## Usage

```bash
$ cd to/thelia/repository
$ php Thelia
```

If you use the command line without any argument, it will display all command
and options available.

List of available commands :

| **command**  | **description**  | **example** |
|---|---|---|
| admin:create  | Create a new administrator user  | ``` $ php Thelia admin:create```  |
| admin:updatePassword  | Change administrator password  | ``` $ php Thelia admin:updatePassword adminlogin [--pasword="..."] ```  |
| cache:clear | Invalidate cache | ``` $ php Thelia cache:clear [--env="..."] [--without-assets] [--with-images]``` |
| image-cache:clear | Empty part or whole web space image cache | ```$ php Thelia  image-cache:clear [subdir]``` |
| module:activate | Activate a module | ```$ php Thelia module:activate module-name ``` |
| module:deactivate | Deactivate a module | ```$ php Thelia module:deactivate module-name ``` |
| module:generate | Generate all needed files for creating a new Module | ```$ php Thelia module:generate module-name ``` |
| module:generate:model | Generate model for a specific module | ``` $ php Thelia module:generate:model module-name [--generate-sql]``` |
| module:generate:sql | Generate the sql from schema.xml file for a specific module | ```$ php Thelia module:generate:sql module-name``` |
| module:refresh | Refresh module list | ```$ php Thelia module:refresh``` |
| thelia:dev:reloadDB | Erase current database and create new one. **all your data will be lost** | ```$ php Thelia thelia:dev:reloadDB``` |
| thelia:generate-resources |  Outputs admin resources | ```$ php Thelia thelia:generate-resources [--output[="..."]]``` |
| thelia:install | Install Thelia | ```$ php Thelia thelia:install``` |
| thelia:update | Update Thelia database. Before doing that you have to update Thelia files | ```$ php Thelia thelia:update``` |

