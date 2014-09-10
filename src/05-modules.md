# Modules

Modules are the best way to extend Thelia functionalities. Payment and delivery methods are all modules.

A module has the exact same structure as Thelia's core and can interact with the container for adding its own services,
creating new compilers, etc.

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
|           | On the export node, id, class and category_id properties are mandatories. id is an unique identifier     |
|           | and class the full path to the class.                                                                    |
|           |                                                                                                          |
|           | category_id possible values are :                                                                        |
|           |                                                                                                          |
|           | * thelia.export.customer : Exports concerning customers                                                  |
|           | * thelia.export.products : Exports concerning products                                                   |
|           | * thelia.export.content : Exports concerning contents                                                    |
|           | * thelia.export.order : Exports concerning orders                                                        |
|           | * thelia.export.modules : module related exports                                                         |
|           |                                                                                                          |
|           |                                                                                                          |
|           | You can also create a custom category if you want. For this you have to put something like below :       |
|           |                                                                                                          |
|           | ```xml                                                                                                   |
|           | <export_categories>                                                                                      |
|           |     <export_category id="your.category.id">                                                              |
|           |         <title locale="en_US">A title</title>                                                            |
|           |         <title locale="fr_FR">Un titre</title>                                                           |
|           |     </export_category>                                                                                   |
|           |     <export_category id="your.other.category.id">                                                        |
|           |         <!-- here's another import category -->                                                          |
|           |     </export_category>                                                                                   |
|           | </export_categories>                                                                                     |
|           | ```                                                                                                      |
|           |                                                                                                          |
|           |                                                                                                          |
+-----------+----------------------------------------------------------------------------------------------------------+
|           |                                                                                                          |
| import    | ```xml                                                                                                   |
|           | <import id="your.import.id" class="Your\ImportHandler" category_id="the.category_id">                    |
|           |     <descriptive locale="en_US">                                                                         |
|           |         <title>Your import title </title>                                                                |
|           |          <!-- you may add an optionnal description -->                                                   |
|           |          <description> ... </description>                                                                |
|           |     </descriptive>                                                                                       |
|           |     <descriptive locale="fr_FR">                                                                         |
|           |         <!-- Here's for another locale -->                                                               |
|           |     </descriptive>                                                                                       |
|           | </import>                                                                                                |
|           | ```                                                                                                      |
|           |                                                                                                          |
|           | On the import node, id, class and category_id properties are mandatories. id is an unique identifier     |
|           | and class the full path to the class.                                                                    |
|           |                                                                                                          |
|           | category_id possible values are :                                                                        |
|           |                                                                                                          |
|           | * thelia.import.customer : Imports concerning customers                                                  |
|           | * thelia.import.products : Imports concerning products                                                   |
|           | * thelia.import.content : Imports concerning contents                                                    |
|           | * thelia.import.order : Imports concerning orders                                                        |
|           | * thelia.import.modules : module related imports                                                         |
|           |                                                                                                          |
|           |                                                                                                          |
+-----------+----------------------------------------------------------------------------------------------------------+

## Module.xml content

Module.xml file is a description of your module. It contains who is the author and how to contact him, module version
and with which version of Thelia it works.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<module>
    <fullnamespace>Atos\Atos</fullnamespace>
    <descriptive locale="en_US">
        <title>Atos-sips payment module</title>
    </descriptive>
    <descriptive locale="fr_FR">
        <title>module de paiement Atos-sips</title>
    </descriptive>
    <version>0.9</version>
    <author>
        <name>Manuel Raynaud</name>
        <email>manu@thelia.net</email>
    </author>
    <type>payment</type>
    <thelia>2.0.0</thelia>
    <stability>beta</stability>
</module>
```

+---------------+------------------------------------------------------------------------------------------------------+
|Tag            |Description                                                                                           |
+===============+======================================================================================================+
|fullnamespace  | The full namespace for the module's main class.                                                      |
+---------------+------------------------------------------------------------------------------------------------------+
|descriptive    | This block can be repeat for how many locales you want. It contains a title, subtitle, description   |
|               | and postscriptum. Only the title is mandatory.                                                       |
+---------------+------------------------------------------------------------------------------------------------------+
|version        | Module version                                                                                       |
+---------------+------------------------------------------------------------------------------------------------------+
|author         | Author information. It contains a name, a company, an email and a website tag. Only the name is      |
|               | mandatory                                                                                            |
+---------------+------------------------------------------------------------------------------------------------------+
|type           | What type of your module. Can be one of the value below :                                            |
|               |                                                                                                      |
|               | * payment : your module is a payment gateway.                                                        |
|               | * delivery : your module is a delivery platform.                                                     |
|               | * classic : all other types.                                                                         |
+---------------+------------------------------------------------------------------------------------------------------+
|thelia         | Which version of Thelia your module is compatible for.                                               |
+---------------+------------------------------------------------------------------------------------------------------+
|stability      | You module stability. Can be one of the value below :                                                |
|               |                                                                                                      |
|               | * alpha                                                                                              |
|               | * beta                                                                                               |
|               | * rc                                                                                                 |
|               | * prod                                                                                               |
|               | * other                                                                                              |
|               |                                                                                                      |
+---------------+------------------------------------------------------------------------------------------------------+
