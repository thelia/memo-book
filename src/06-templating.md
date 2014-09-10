# Templating

Thelia templates uses the Smarty template engine, enriched by many Thelia additions, such as loops, data access
functions, internationalization function, etc

## Structure

See Thelia structure for more information.

Every template should contains specific template files, which are the views invoked in the Front and Back
Offices controllers.
For a front-office template, these files are :

* **product.html** : display a product.
* **content.html** : display a contents.
* **category.html** : displays a category contents.
* **feed.html** : the RSS product or content feed.
* **folder.html** : display a folder contents.
* **404.html** : displayed if a page cannot be found.
* **order-delivery.html** : display for choosing a delivery method.
* **order-failed.html** : display when a payment failed.
* **order-invoice.html** : display for choosing a payment gateway.
* **order-payment-gateway.html** : fill by the payment gateway for sending a specific form to the platform.
* **order-placed.html** : display for a success payment.


## Assets management

Template assets are managed in a sub-directory of the template directory. For example, the default front-office
template contains an 'assets' directory to store all template's assets.

To use this feature, you'll have to add some specific directives to your template files.

### {declare_assets}

This directive tells the Thelia template system where your assets are located, e.g. the name of the root directory
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

This block return only one parameter, $asset_url, which is the asset URL in the web space, e.g. under the web/assets path.

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

This directive process your statics images used in your template.

Example :
```JavaScript
{images file='assets/img/favicon.ico'}
    <link rel="shortcut icon" type="image/x-icon" href="{$asset_url}">
{/images}
```
This block return only one parameter, $asset_url, which is the asset URL in the web space, e.g. under the web/assets path.

List of parameters

+---------------------------+------------------------------------------------------------------------------------------+
|Parameter                  |Description                                                                               |
+===========================+==========================================================================================+
|file                       | This is the path to the file (jokers like '*' are **NOT** allowed),                      |
|                           | relative to the template base path.                                                      |
+---------------------------+------------------------------------------------------------------------------------------+
|source                     | When in the templates files of a module, use this parameter to specify that the source of|
|                           | the asset has to be searched within the module's path instead of the main template path. |
+---------------------------+------------------------------------------------------------------------------------------+
|template                   | You may want to use an asset located in another template of the same type (for example,  |
|                           | another front office template). To do so, specify the name of this template in the       |
|                           | template parameter                                                                       |
+---------------------------+------------------------------------------------------------------------------------------+

### {javascripts}

This directive process your javascript files

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
|source                     | When in the templates files of a module, use this parameter to specify that the source of|
|                           | the asset has to be searched within the module's path instead of the main template path. |
+---------------------------+------------------------------------------------------------------------------------------+
|template                   | You may want to use an asset located in another template of the same type (for example,  |
|                           | another front office template). To do so, specify the name of this template in the       |
|                           | template parameter                                                                       |
+---------------------------+------------------------------------------------------------------------------------------+

## Internationalization

If you want to create multilingual compatible templates, you have to pay a special attention to : - static text -
date formatting - number formatting - money formatting

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
|l          | The l parameter contains the string that will be translated. This string should not contains any         |
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
|           | For examples, in the layout.tpl file of the default front-office template, you'll find                   |
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

List fo parameters

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

Use this function to format a number according to the current locale standards, or a specific format.

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
|decimals       | How many decimals format expected. If omitted, the current locale parameter is taken                 |
+---------------+------------------------------------------------------------------------------------------------------+
|dec_point      | Separator for the decimal point. If omitted, the current locale parameter is taken                   |
+---------------+------------------------------------------------------------------------------------------------------+
|thousands_sep  | Thousands separator. If omitted, the current locale parameter is taken                               |
+---------------+------------------------------------------------------------------------------------------------------+
