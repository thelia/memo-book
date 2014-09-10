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

### Config.xml content

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

### Module.xml content

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


## Routing

Thelia uses the symfony-cmf routing component, so it's possible to declare how many routers needed and add them in this
routing component.
If you need a router you can do it in two different ways

### The default behavior

All you have to do is to create a file named routing.xml in your Config directory. Thelia will configure a new router
and set a default priority (150) to it.

### Custom routing

If you need a custom configuration for your routing, you can declare a new service and tag this service and put
```router.register``` for the name property and the priority you want.

Here an example :

```xml
<service id="router.front" class="%router.class%">
    <argument type="service" id="router.module.xmlLoader"/>
    <argument>Front/Config/front.xml</argument>
    <argument type="collection">
        <argument key="cache_dir">%kernel.cache_dir%</argument>
        <argument key="debug">%kernel.debug%</argument>
    </argument>
    <argument type="service" id="request.context"/>
    <tag name="router.register" priority="128"/>
</service>
```

## Model

Your module will maybe need to create tables, generate model classes and interact with Thelia's model.

1. Create the file schema.xml in your Config directory.
2. Fill schema.xml file, you can find all the information you need in the propel documentation.
3. Use the CLI tools for generating model and sql (```php Thelia module:generate:model MyModule --generate-sql```).

Note : it's better to put the namespace property on each table attributes instead of the database attribute.

## Main class

The main class in your module is the most important file. This class is used when the module is activated or
deactivated.

Most of time this class will have the same name as you module directory. If my module directory is **Atos**, my
main class will be **Atos** too and the full namespace will be **Atos\\Atos**.

Depending of the type of your module, this class must extends a specific abstract class. Here is a list of all
abstract classes :

* **Thelia\\Module\\AbstractDeliveryModule** : Use this class when you develop a delivery module
* **Thelia\\Module\\AbstractPaymentModule** : Use this class when you develop a payment module
* **Thelia\\Module\\BaseModule** : Use this class when you develop a class module

**AbstractDeliveryModule** and **AbstractPaymentModule** classes extends BaseModule class.

Some methods in **BaseModule** can be useful if you want to interact with Thelia during installation or removal process.
You just have to overload the method you want and implement your code.

+-----------------+----------------------------------------------------------------------------------------------------+
|Method           |Description                                                                                         |
+=================+====================================================================================================+
|preActivation    | This method is called before the module activation, and may prevent it by returning false.         |
+-----------------+----------------------------------------------------------------------------------------------------+
|postActivation   | This method is called just after the module was successfully activated. If an exception is thrown  |
|                 | the procedure will be stopped and a rollback of the current transaction will be performed.         |
+-----------------+----------------------------------------------------------------------------------------------------+
|preDeactivation  | This method is called before the module de-activation, and may prevent it by returning false.      |
+-----------------+----------------------------------------------------------------------------------------------------+
|postDeactivation | This method is called just after the module was successfully deactivated. If an exception is thrown|
|                 | the procedure will be stopped and a rollback of the current transaction will be performed.         |
+-----------------+----------------------------------------------------------------------------------------------------+
|getCompilers     | This method adds new compilers to Thelia container                                                 |
+-----------------+----------------------------------------------------------------------------------------------------+
|getHooks         | This method must be used when your module defines hooks.                                           |
+-----------------+----------------------------------------------------------------------------------------------------+

Specific methods for **AbstractDeliveryModule**

+-----------------+----------------------------------------------------------------------------------------------------+
|Method           |Description                                                                                         |
+=================+====================================================================================================+
|isValidDelivery  | This method is called by the Delivery loop, to check if the current module has to be displayed     |
|                 | to the customer. This method must be implemented in your module                                    |
+-----------------+----------------------------------------------------------------------------------------------------+
|getPostage       | Calculate and return delivery price in the shop's default currency. This method must be implemented|
|                 | in your module                                                                                     |
+-----------------+----------------------------------------------------------------------------------------------------+

Specific methods for **AbstractPaymentModule**

+---------------------------+------------------------------------------------------------------------------------------+
|Method                     |Description                                                                               |
+===========================+==========================================================================================+
|pay                        | Method used by payment gateway. This method must be implemented in your module           |
+---------------------------+------------------------------------------------------------------------------------------+
|isValidPayment             | This method is call on Payment loop, to check if the current module has to be displayed  |
|                           | to the customer. This method must be implemented in your module                          |
+---------------------------+------------------------------------------------------------------------------------------+
|generateGatewayFormResponse| Render the payment gateway template. The module should provide the gateway URL           |
|                           | and  the form fields names and values. This method is a helper                            |
+---------------------------+------------------------------------------------------------------------------------------+
|getPaymentSuccessPageUrl   | Return the order payment success page URL                                                |
+---------------------------+------------------------------------------------------------------------------------------+
|getPaymentFailurePageUrl   | Redirect the customer to the failure payment page. if $message is null,                  |
|                           | a generic message is displayed.                                                          |
+---------------------------+------------------------------------------------------------------------------------------+