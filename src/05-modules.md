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

------------------------------------------------------------------------------------------------------------------------
   Tag      Description
----------- ------------------------------------------------------------------------------------------------------------
   loop     declare a loop. name and class properties are mandatories. The name is a unique key and class the full
            namespace for the loop class.

   form     declare a form. name and class properties are mandatories. The name is a unique key and class the full
            namespace for the form class.

   command  declare a command. name property is mandatory. The class is the full namespace for the command class.

------------------------------------------------------------------------------------------------------------------------



