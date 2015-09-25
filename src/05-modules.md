# Modules

Modules are the best way to extend Thelia functionalities. Payment and delivery methods are all modules.

The structure of a module is exactly the same as Thelia's core. A module can interact with the container in order
to add its own services, to create new compilers, etc.

Thelia have a cli command to generate all needed files for creating a new Module : ```php Thelia generate:module MyModule```


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
|   loop    | Declare a loop. Name and class properties are mandatory. The name is a unique key and the class is the   |
|           | full namespace for the loop class.                                                                       |
+-----------+----------------------------------------------------------------------------------------------------------+
|   form    | Declare a form. Name and class properties are mandatory. The name is a unique key and the class is the   |
|           | full namespace for the form class.                                                                       |
+-----------+----------------------------------------------------------------------------------------------------------+
|   command | Declare a command. Name property is mandatory. The class is the full namespace for the command class.    |
+-----------+----------------------------------------------------------------------------------------------------------+
|   service | Services are the exact same notion as for Symfony services. See the dedicated chapter below              |
+-----------+----------------------------------------------------------------------------------------------------------+
|   hook    | Hooks are the entry points thanks to which modules will insert their own code. To configure hooks, you   |
|           | must declare them in the config.xml file. Example :                                                      |
|           | ```xml                                                                                                   |
|           | <hook id="mymodule.hook" class="MyModule\Hook\MySuperHook" scope="request">                              |
|           |     <tag name="hook.event_listener" event="main.body.bottom" type="front" method="onMainBodyBottom" />   |
|           | </hook>                                                                                                  |
|           | ```                                                                                                      |
|           |                                                                                                          |
|           | On the hook node, id and class are mandatory. The **id** is a unique identifier and the **class** is the |
|           | full path to the class.                                                                                  |
|           |                                                                                                          |
|           | On the tag node, name and event are mandatory. The others are not mandatory, here are more details :     |
|           |                                                                                                          |
|           | * ```name="hook.event_listener"``` : this never changes.                                                 |
|           | * event : represents the hook code to which it wants to respond.                                         |
|           | * type : indicates the context of the hook : frontOffice (default), backOffice, pdf or email.            |
|           | * method : indicates the name of the method to call. By default, it will be based on the name of the hook|
|           |  . eg : for product.additional hook, the method will be called onProductAdditional                       |
|           |   (CamelCase prefixed by on).                                                                            |
|           | * active : allows you to activate the hook (set to 1 - default) or not (set to 0) once the module        |
|           |   is installed                                                                                           |
+-----------+----------------------------------------------------------------------------------------------------------+
|   export  |                                                                                                          |
|           | ```xml                                                                                                   |
|           | <export id="your.export.id" class="Your\ExportHandler" category_id="the.category_id">                    |
|           |   <descriptive locale="en_US">                                                                           |
|           |       <title>Your export title </title>                                                                  |
|           |       <!-- you may add an optional description -->                                                      |
|           |       <description> ... </description>                                                                   |
|           |   </descriptive>                                                                                         |
|           |   <descriptive locale="fr_FR">                                                                           |
|           |       <!-- Here's for another locale -->                                                                 |
|           |   </descriptive>                                                                                         |
|           | </export>                                                                                                |
|           | ```                                                                                                      |
|           |                                                                                                          |
|           | On the export node, **id**, **class** and **category_id** properties are mandatory. The **id** is a      |
|           | unique identifier and the class is the full path to the class.                                           |
|           |                                                                                                          |
|           | **category_id** possible values are :                                                                    |
|           |                                                                                                          |
|           | * thelia.export.customer : Exports the customers' data                                                   |
|           | * thelia.export.products : Exports the products' data                                                    |
|           | * thelia.export.content : Exports the contents' data                                                     |
|           | * thelia.export.order : Exports the orders' data                                                         |
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
|           |          <!-- you may add an optional description -->                                                   |
|           |          <description> ... </description>                                                                |
|           |     </descriptive>                                                                                       |
|           |     <descriptive locale="fr_FR">                                                                         |
|           |         <!-- Here's for another locale -->                                                               |
|           |     </descriptive>                                                                                       |
|           | </import>                                                                                                |
|           | ```                                                                                                      |
|           |                                                                                                          |
|           | On the import node, **id**, **class** and **category_id** properties are mandatory. The **id** is a      |
|           | unique identifier and the **class** is the full path to the class.                                       |
|           |                                                                                                          |
|           | category_id possible values are :                                                                        |
|           |                                                                                                          |
|           | * thelia.import.customer : Imports the customers' data                                                   |
|           | * thelia.import.products : Imports the products' data                                                    |
|           | * thelia.import.content : Imports the contents' data                                                     |
|           | * thelia.import.order : Imports the orders' data                                                         |
|           | * thelia.import.modules : module related imports                                                         |
|           |                                                                                                          |
|           |                                                                                                          |
+-----------+----------------------------------------------------------------------------------------------------------+

### Module.xml content

Module.xml file is a description of your module. It includes the author's name, his contact details, module version
and the version of Thelia it is compatible with.

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
|fullnamespace  | The full namespace of the module's main class.                                                       |
+---------------+------------------------------------------------------------------------------------------------------+
|descriptive    | This block can be repeated for as many locale as you want. It includes a title, subtitle,            |
|               | description and postscriptum. Only the title is mandatory.                                           |
+---------------+------------------------------------------------------------------------------------------------------+
|version        | Module version                                                                                       |
+---------------+------------------------------------------------------------------------------------------------------+
|author         | Author information. It includes a name, a company, an email and a website tag. Only the name is      |
|               | mandatory                                                                                            |
+---------------+------------------------------------------------------------------------------------------------------+
|type           | The type of your module. It can be :                                                                 |
|               |                                                                                                      |
|               | * payment : your module is a payment gateway.                                                        |
|               | * delivery : your module is a delivery platform.                                                     |
|               | * classic : all other types of modules.                                                              |
+---------------+------------------------------------------------------------------------------------------------------+
|thelia         | Which version of Thelia your module is compatible with.                                              |
+---------------+------------------------------------------------------------------------------------------------------+
|stability      | Your module stability. Can be one of the value below :                                               |
|               |                                                                                                      |
|               | * alpha                                                                                              |
|               | * beta                                                                                               |
|               | * rc                                                                                                 |
|               | * prod                                                                                               |
|               | * other                                                                                              |
|               |                                                                                                      |
+---------------+------------------------------------------------------------------------------------------------------+


## Routing

Thelia uses the Symfony-cmf Routing component, so it's possible to declare as many routers as are needed and add them in
this routing component.
If you need to add a router you can do it in two different ways

### The default behavior

All you have to do is to create a file named routing.xml in your Config directory. Thelia will configure a new router
and set a default priority (150) to it.

### Custom routing

If you need a custom configuration for your routing, you can declare a new service and tag this service and put
```router.register``` for the name property and the priority you want.

Here is an example :

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

Your module may need to create tables, generate model classes and interact with Thelia's model. How to do ?

1. Create the file schema.xml in your Config directory.
2. Fill schema.xml file, you can find all the information you need in Propel documentation.
3. Use the CLI tools to generate model and sql (```php Thelia module:generate:model MyModule --generate-sql```).

Note : it's better to put the namespace property on each table attribute instead of the database attribute.

## Main class

The main class in your module is the most important file. This class is used when the module is activated or
deactivated.

Most of the time this class will have the same name as your module directory. If my module directory is **Atos**, my
main class will be **Atos** too and the full namespace will be **Atos\\Atos**.

Depending of the type of your module, this class must extend a specific abstract class. Here is a list of all
abstract classes :

* **Thelia\\Module\\AbstractDeliveryModule** : Use this class when you develop a delivery module
* **Thelia\\Module\\AbstractPaymentModule** : Use this class when you develop a payment module
* **Thelia\\Module\\BaseModule** : Use this class when you develop a classic module

**AbstractDeliveryModule** and **AbstractPaymentModule** classes extend the BaseModule class.

Some methods in **BaseModule** can be useful if you want to interact with Thelia during the installation or the removal
process. You just have to overload the method you want and implement your code.

+-----------------+----------------------------------------------------------------------------------------------------+
|Method           |Description                                                                                         |
+=================+====================================================================================================+
|preActivation    | This method is called before the module activation, and may prevent it by returning false.         |
+-----------------+----------------------------------------------------------------------------------------------------+
|postActivation   | This method is called just after the module was successfully activated. If an exception is thrown  |
|                 | the procedure will be stopped and a rollback of the current transaction will be performed.         |
+-----------------+----------------------------------------------------------------------------------------------------+
|preDeactivation  | This method is called before the module deactivation, and may prevent it by returning false.      |
+-----------------+----------------------------------------------------------------------------------------------------+
|postDeactivation | This method is called just after the module was successfully deactivated. If an exception is thrown|
|                 | the procedure will be stopped and a rollback of the current transaction will be performed.         |
+-----------------+----------------------------------------------------------------------------------------------------+
|getCompilers     | This method adds new compilers to Thelia container                                                 |
+-----------------+----------------------------------------------------------------------------------------------------+
|getHooks         | This method must be used if your module defines hooks.                                             |
+-----------------+----------------------------------------------------------------------------------------------------+

Specific methods for **AbstractDeliveryModule**

+-----------------+----------------------------------------------------------------------------------------------------+
|Method           |Description                                                                                         |
+=================+====================================================================================================+
|isValidDelivery  | This method is called by the Delivery loop, to check if the current module has to be displayed     |
|                 | to the customer. This method must be implemented in your module                                    |
+-----------------+----------------------------------------------------------------------------------------------------+
|getPostage       | This method calculates and returns the delivery price. This method must be implemented             |
|                 | in your module                                                                                     |
+-----------------+----------------------------------------------------------------------------------------------------+

Specific methods for **AbstractPaymentModule**

+---------------------------+------------------------------------------------------------------------------------------+
|Method                     |Description                                                                               |
+===========================+==========================================================================================+
|pay                        | Method used by payment gateways. This method must be implemented in your module          |
+---------------------------+------------------------------------------------------------------------------------------+
|isValidPayment             | This method is called by the Payment loop, to check if the current module has to be      |
|                           | displayed to the customer. This method must be implemented in your module                |
+---------------------------+------------------------------------------------------------------------------------------+
|generateGatewayFormResponse| This method renders the payment gateway template. The module should provide the gateway  |
|                           | URL and  the form fields names and values. This method is a helper                       |
+---------------------------+------------------------------------------------------------------------------------------+
|getPaymentSuccessPageUrl   | Return the order payment success page URL                                                |
+---------------------------+------------------------------------------------------------------------------------------+
|getPaymentFailurePageUrl   | Redirect the customer to the failure payment page. If $message is null,                  |
|                           | a generic message is displayed.                                                          |
+---------------------------+------------------------------------------------------------------------------------------+
