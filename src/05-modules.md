# Modules

Modules are the best way to extend Thelia functionalities. Payment and delivery methods are all modules.

A module has the exact same structure as Thelia's core and can interact with the container for adding its own services,
create new compilers, etc.

## Structure

```
\MyModule
    \Config
        config.xml   <- mandatory
        module.xml   <- mandatory
        routing.xml
        schema.xml
    MyModule.php <- mandatory
    \Loop
        Product.php
        MyLoop.php
    ...
```

## Config.xml content

```xml
<?xml version="1.0" encoding="UTF-8" ?>

<config xmlns="http://thelia.net/schema/dic/config"
        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xsi:schemaLocation="http://thelia.net/schema/dic/config http://thelia.net/schema/dic/config/thelia-1.0.xsd">

        <loops>
            <loop name="MySuperLoop" class="MyModule\Loop\MySuperLoop" />
        </loops>

        <forms>
            <form name="MyFormName" class="MyModule\Form\MySuperForm" />
        </forms>

        <commands>
            <command class="MyModule\Command\MySuperCommand" />
        </commands>

        <services>
            <service id="Mymodule.service.id" class="MyModule\MySuperService"/>
        </services>
        -->

        <hooks>
            <hook id="mymodule.hook" class="MyModule\Hook\MySuperHook" scope="request">
                <tag name="hook.event_listener" event="main.body.bottom" type="front|back|pdf|email" method="onMainBodyBottom" />
            </hook>
        </hooks>

        <exports> </exports>

        <imports> </imports>

</config>
```

+-----------+----------------------------------------------------------------------------------------------------------+
|Tag        |Description                                                                                               |
+===========+==========================================================================================================+
|   loop    | declare a loop. name and class properties are mandatories. The name is a unique key and class the full   |
|           | namespace for the loop class.                                                                            |
+-----------+----------------------------------------------------------------------------------------------------------+
|   form    | declare a form. name and class properties are mandatories. The name is a unique key and class the full   |
|           | namespace for the form class.                                                                            |
+-----------+----------------------------------------------------------------------------------------------------------+
|   command | declare a command. name property is mandatory. The class is the full namespace for the command class.    |
+-----------+----------------------------------------------------------------------------------------------------------+
|   service | Services are the exact same notion as symfony services. See de dedicated chapter below                   |
+-----------+----------------------------------------------------------------------------------------------------------+
|   hook    | Hooks are entry point in your template in which the modules will insert their own code. For configuring a|
|           | hook, you must declare them in the config.xml file. Example :                                            |
|           | ```xml                                                                                                   |
|           | <hook id="mymodule.hook" class="MyModule\Hook\MySuperHook" scope="request">                              |
|           |     <tag name="hook.event_listener" event="main.body.bottom" type="front" method="onMainBodyBottom" />   |
|           | </hook>                                                                                                  |
|           | ```                                                                                                      |
|           |                                                                                                          |
|           | On the hook node, id and class are mandatories. id is an unique identifier and class the full path to the|
|           | class.                                                                                                   |
|           |                                                                                                          |
|           | On the tag node, name and event are mandatories, the other are not and are explain just below :          |
|           |                                                                                                          |
|           | * ```name="hook.event_listener"``` : this never change                                                   |
|           | * event : represents the hook code for which it wants to respond.                                        |
|           | * type : indicate the context of the hook : frontOffice (default), backOffice, pdf or email.             |
|           | * method : indicate the method to be called. By default, it will be based on the name of the hook.       |
|           |   eg : for product.additional hook, the method onProductAdditional will be called                        |
|           |   (CamelCase prefixed by on).                                                                            |
|           | * active : allow you to activate the hook (set to 1 - default) or not (set to 0) when the module         |
|           |   is installed                                                                                           |
+-----------+----------------------------------------------------------------------------------------------------------+
|   export  |                                                                                                          |
|           | ```xml                                                                                                   |
|           | <export id="your.export.id" class="Your\ExportHandler" category_id="the.category_id">                    |
|           |   <descriptive locale="en_US">                                                                           |
|           |       <title>Your export title </title>                                                                  |
|           |       <!-- you may add an optionnal description -->                                                      |
|           |       <description> ... </description>                                                                   |
|           |   </descriptive>                                                                                         |
|           |   <descriptive locale="fr_FR">                                                                           |
|           |       <!-- Here's for another locale -->                                                                 |
|           |   </descriptive>                                                                                         |
|           | </export>                                                                                                |
|           | ```                                                                                                      |
|           |                                                                                                          |
|           |                                                                                                          |
|           |                                                                                                          |
+-----------+----------------------------------------------------------------------------------------------------------+

