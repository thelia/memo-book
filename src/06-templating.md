# Templating

Thelia templates use the Smarty template engine, enriched by many Thelia additions, such as loops, data access
functions, internationalization function, etc

## Structure

See Thelia structure for more information.

Every template should contain specific template files, which are the views invoked in the Front and Back
Offices controllers.
For a front-office template, these files are :

* **product.html** : displays a product.
* **content.html** : displays a content.
* **category.html** : displays a category's content.
* **feed.html** : the RSS product or content feed.
* **folder.html** : display a folder content.
* **404.html** : displayed if a page cannot be found.
* **order-delivery.html** : is displayed during ordering process to choose a delivery method
* **order-invoice.html** : is displayed during ordering process to choose a payment gateway.
* **order-failed.html** : is displayed when a payment failed.
* **order-payment-gateway.html** : filled by the payment gateway to send to the platform a specific form.
* **order-placed.html** : is displayed once the payment si successfully performed


## Assets management

Template assets are managed in a sub-directory of the template directory. For example, the default front-office
template contains an 'assets' directory to store all template's assets.

To use this feature, you'll have to add some specific directives to your template files.

### {declare_assets}

This directive tells Thelia's template system where your assets are located, e.g. the name of the root directory
which contains all your assets.

Example :
```JavaScript
{declare_assets directory="assets"}
```

### {stylesheets}

This directive processes your CSS style sheets.

Example :

```JavaScript
{stylesheets file="assets/css/*.less" filters="less"}
    <link href="{$asset_url}" rel="stylesheet" type="text/css" />
{/stylesheets}
```

This block returns only one parameter, $asset_url, which is the asset URL in the web directory, e.g. under the
web/assets path.

List of parameters

+---------------------------+------------------------------------------------------------------------------------------+
|Parameter                  |Description                                                                               |
+===========================+==========================================================================================+
|file                       | This is the path to the file (or files, as jokers like '*' are allowed),                 |
|                           | relative to the template base path.                                                      |
+---------------------------+------------------------------------------------------------------------------------------+
|filters                    | Apply a filter to the source(s) files. Available filters are :                           |
|                           |                                                                                          |
|                           | * less : compile CSS using the LESS compiler                                             |
|                           | * sass : compile CSS using the SASS compiler                                             |
|                           | * compass : compile CSS using the Compass compiler                                       |
+---------------------------+------------------------------------------------------------------------------------------+
|source                     | When in the templates files of a module, use this parameter to specify that the source of|
|                           | the asset has to be searched within the module's path instead of the main template path. |
+---------------------------+------------------------------------------------------------------------------------------+
|template                   | You may want to use an asset located in another template of the same type (for example,  |
|                           | another front office template). To do so, specify the name of this template in the       |
|                           | template parameter                                                                       |
+---------------------------+------------------------------------------------------------------------------------------+

### {images}

This directive processes the static images used in your template.

Example :
```JavaScript
{images file='assets/img/favicon.ico'}
    <link rel="shortcut icon" type="image/x-icon" href="{$asset_url}">
{/images}
```
This block returns only one parameter, $asset_url, which is the asset URL in the web directory, e.g. under the
web/assets path.

List of parameters

+---------------------------+------------------------------------------------------------------------------------------+
|Parameter                  |Description                                                                               |
+===========================+==========================================================================================+
|file                       | This is the path to the file (jokers like '*' are **NOT** allowed),                      |
|                           | relative to the template base path.                                                      |
+---------------------------+------------------------------------------------------------------------------------------+
|source                     | When the asset is in a module directory, you need to use this parameter to specify what  |
|                           | the source of the asset has to be searched within the module's path instead of the main  |
|                           | template path.                                                                           |
+---------------------------+------------------------------------------------------------------------------------------+
|template                   | You may want to use an asset located in another template of the same type (for example,  |
|                           | another front office template). To do so, specify the name of this template in the       |
|                           | template parameter                                                                       |
+---------------------------+------------------------------------------------------------------------------------------+

### {javascripts}

This directive processes your javascript files

Example :

```JavaScript
{javascripts file='assets/js/script.js'}
    <script type="text/javascript" src="{$asset_url}"></script>
{/javascripts}
```

List of parameters

+---------------------------+------------------------------------------------------------------------------------------+
|Parameter                  |Description                                                                               |
+===========================+==========================================================================================+
|file                       | This is the path to the file (or files, as jokers like '*' are allowed),                 |
|                           | relative to the template base path.                                                      |
+---------------------------+------------------------------------------------------------------------------------------+
|source                     | When the asset is in a module directory, you need to use this parameter to specify what  |
|                           | the source of the asset has to be searched within the module's path instead of the main  |
|                           | template path.                                                                           |
+---------------------------+------------------------------------------------------------------------------------------+
|template                   | You may want to use an asset located in another template of the same type (for example,  |
|                           | another front office template). To do so, specify the name of this template in the       |
|                           | template parameter                                                                       |
+---------------------------+------------------------------------------------------------------------------------------+

## Internationalization

If you want to create multilingual compatible templates, you have to pay special attention to : - static text -
date formatting - number formatting

Thelia provides several Smarty functions to help you.

### {intl}

The {intl} function translates a string into the current language.

Example :

```
{intl l="This is a string to translate"}
```

List of parameters

+-----------+----------------------------------------------------------------------------------------------------------+
|Parameter  |Description                                                                                               |
+===========+==========================================================================================================+
|l          | The l parameter contains the string that will be translated. This string should not contain any          |
|           | variable, such as {intl l="Hello, $name, how do you do ?"}, internal variables should be used instead.   |
|           | Every %varname found in the string will be replaced by the value of the varname parameter. For example:  |
|           | {intl l="Hello, %user, how do you do ?" user=$name} is fine.                                             |
|           |                                                                                                          |
|           | If no translation can be found for a given string, the translator will return either the value of the l  |
|           | parameter, or an empty string, depending on the "Languages & URLs" parameters.                           |
+-----------+----------------------------------------------------------------------------------------------------------+
|d          | The d parameter is the message domain, a set of internationalized messages. Thelia contains the          |
|           | following domains :                                                                                      |
|           |                                                                                                          |
|           | * core : for Thelia core translations                                                                    |
|           | * bo.*template_name* (eg : bo.default) : for each back-office template                                   |
|           | * fo.*template_name* (eg : fo.default) : for each front-office template                                  |
|           | * pdf.*template_name* (eg : pdf.default) : for each PDF template                                         |
|           | * email.*template_name* (eg : email.default) : for each email template                                   |
|           | * in modules :                                                                                           |
|           |       * *module_code* (eg : paypal) : for module core translations                                       |
|           |       * *module_code*.ai (eg : paypal.ai) : used in AdminIncludes templates                              |
|           |       * *module_code*.bo.*template_name* (eg : paypal.bo.default) : used in back office template         |
|           |       * *module_code*.fo.*template_name* (eg : paypal.fo.default) : used in front office template        |
|           |                                                                                                          |
|           | This parameter is mostly used in modules. Other templates (front-office, back-office, PDF and email)     |
|           | may use the {default_translation_domain} function to define a template-wide message domain, and the d    |
|           | parameter could then be omitted.                                                                         |
|           |                                                                                                          |
|           | For example, in the layout.tpl file of the default front-office template, you'll find                    |
|           | {default_translation_domain domain='fo.default'}.                                                        |
+-----------+----------------------------------------------------------------------------------------------------------+
|js         | When using {intl} in a Javascript string, the translated string may contain simple and/or double quotes, |
|           | that should be escaped to prevent a syntax error.                                                        |
|           |                                                                                                          |
|           | To do so, use the js parameter, that will escape single and double quotes.                               |
|           | ```JavaScript                                                                                            |
|           |  var myString = '{intl l="A string with 'simple' and \"double\" quotes" js=1}';                          |
|           | ```                                                                                                      |
+-----------+----------------------------------------------------------------------------------------------------------+

### {format_date}

Use this function to format a date according to the current locale standards.

example :

```JavaScript
{format_date date=$dateTimeObject}
```

List of parameters

+-----------+----------------------------------------------------------------------------------------------------------+
|Parameter  |Description                                                                                               |
+===========+==========================================================================================================+
|date       | A DateTime object (required)                                                                             |
+-----------+----------------------------------------------------------------------------------------------------------+
|format     | The expected format. The current locale format will be used if this parameter is empty or missing        |
+-----------+----------------------------------------------------------------------------------------------------------+
|output     | The type of desired ouput, one of :                                                                      |
|           |                                                                                                          |
|           | * date : the date only                                                                                   |
|           | * time : the time only                                                                                   |
|           | * datetime : the date and the time (default)                                                             |
+-----------+----------------------------------------------------------------------------------------------------------+

### {format_number}

Use this function to format a number according to the current locale standards, or to a specific format.

Example :

```JavaScript
Outputs "1246,12" if locale is fr_FR, 1 246.12 if locale is en_US
{format_number number="1246.12"}
```

List fo parameters

+---------------+------------------------------------------------------------------------------------------------------+
|Parameter      |Description                                                                                           |
+===============+======================================================================================================+
|number         | Int or float number. (Required)                                                                      |
+---------------+------------------------------------------------------------------------------------------------------+
|decimals       | Number of decimals expected. If omitted, the current locale parameter is taken                       |
+---------------+------------------------------------------------------------------------------------------------------+
|dec_point      | Separator for the decimal point. If omitted, the current locale parameter is taken                   |
+---------------+------------------------------------------------------------------------------------------------------+
|thousands_sep  | Thousands separator. If omitted, the current locale parameter is taken                               |
+---------------+------------------------------------------------------------------------------------------------------+

## Loop system

Loops are the most convenient feature in Thelia for frontend developers. Already there in Thelia's first version,
they have to be improved for Thelia v2.
Loops allow to gather data from your shop and display them in your front view. In Thelia v2, loops are a Smarty
v3 plugin.

### Syntax

```html
{ifloop rel="my_associated_content_loop"}
    Associated contents for this product :
    <ul>
        {loop type="associated_content" name="my_associated_content_loop" product="12"}
            <li>
                <a href="{$URL}">{$TITLE}</a>
            </li>
        {/loop}
    </ul>
{/ifloop}
{elseloop rel="my_associated_content_loop"}
    No associated content for this product
{/elseloop}
```

### {loop} {/loop}

the loop function have at least two mandatory parameters :

+---------------+------------------------------------------------------------------------------------------------------+
|Parameter      |Description                                                                                           |
+===============+======================================================================================================+
|name           | A unique name used to identify the loop in other functions (ifloop and elseloop)                     |
+---------------+------------------------------------------------------------------------------------------------------+
|type           | The type of a loop is the type of data you want to retrieve. For the complete type list, see Thelia  |
|               | documentation at [http://doc.thelia.net](http://doc.thelia.net)                                      |
+---------------+------------------------------------------------------------------------------------------------------+

Each loop type defines its own parameters, you can search this parameter in Thelia documentation.

### {ifloop}/{elseloop}

{ifloop} and {elseloop} are conditional loops. They allow to define a different behaviour depending on if the a
classic loop displays something or not.

A conditional loop is therefore linked to a classic loop using the rel attribute which must match a classic loop
named attribute.

