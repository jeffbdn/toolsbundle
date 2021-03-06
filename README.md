Installation
============

Step 1: Download the Bundle
---------------------------

Open a command console, enter your project directory and execute the
following command to download the latest stable version of this bundle:

```console
$ composer require jeffbdn/tools-bundle "~1"
```

This command requires you to have Composer installed globally, as explained
in the [installation chapter](https://getcomposer.org/doc/00-intro.md)
of the Composer documentation.

Step 2: Enable the Bundle
-------------------------

Then, enable the bundle by adding it to the list of registered bundles
in the `app/AppKernel.php` file of your project:

```php
<?php
// app/AppKernel.php

// ...
class AppKernel extends Kernel
{
    public function registerBundles()
    {
        $bundles = array(
            // ...

            new JeffBdn\ToolsBundle\JeffBdnToolsBundle(),
        );

        // ...
    }

    // ...
}
```

This bundle comes with several services. In order to make them available for your project,
register the bundle's `services.yml` file in your `config.yml`:

```yaml
# app/config/config.yml
imports:
    ...
    - { resource: "@JeffBdnToolsBundle/Resources/config/services.yml" }
```

Then, add and customise those parameters in your `config.yml`: 
```yaml
# app/config/config.yml
parameters:
      jeffbdn_tools.weather.apikey: 'PASTE HERE YOUR openweathermaps API KEY'
      jeffbdn_tools.weather.refresh: '3 minutes' # or 1 day or 2 days or 1 hour or 2 hours or 1 minute or 2 minutes
```

jeffbdn_tools.weather.apikey:
In order to use the Weather Service, you will need an OpenWeatherMaps API Key.
You can get one for free by creating a new account on https://home.openweathermap.org/users/sign_up .

jeffbdn_tools.weather.refresh:
You can choose the refresh rate for the broadcast for all locations.
The cache will be renewed on next call if the data get older than the amount of time your precised. 
 
Step 3: Use the Tools
-------------------------

The current version of JeffBdnToolsBundle provides 2 services:

- A random number provider
```php
$randomNumber = $this->get('jeffbdn_tools.math')->random();
```

- A weather broadcast provider
```php
$weatherBroadcast = $this->get('jeffbdn_tools.weather')->broadcast('Paris,fr');
```

The data you may be interested to use are:

- Temperatures in Kelvin, Celsius, Fahrenheit
```php
$weatherBroadcast['temp_k']
$weatherBroadcast['temp_k_min']
$weatherBroadcast['temp_k_max']
```
```php
$weatherBroadcast['temp_c']
$weatherBroadcast['temp_c']
$weatherBroadcast['temp_c_max']
```
```php
$weatherBroadcast['temp_f']
$weatherBroadcast['temp_f_min']
$weatherBroadcast['temp_f_max']
```
- Sky and Atmosphere details:
```php
$weatherBroadcast['humidity']
$weatherBroadcast['sky_description_short']
$weatherBroadcast['sky_description_long']
$weatherBroadcast['pressure_hpa']
$weatherBroadcast['wind_speed_metersec']
$weatherBroadcast['cloud_percent']
```
- Date of last update, sunrise and sunset:
```php
$weatherBroadcast['date']
$weatherBroadcast['sunrise']
$weatherBroadcast['sunset']
```
- Error Management:
```php
// true if API call went well, else false
$weatherBroadcast['ok']
// this is the HTTP response code, default is 200
$weatherBroadcast['error_code']
// this is the API response message, default is an empty string
$weatherBroadcast['error_string']
```

Step 4: Testing the ToolsBundle
-------------------------
```bash 
$ phpunit vendor/jeffbdn/tools-bundle/JeffBdn/ToolsBundle/Tests/Weather.php
```
If you encounter any bug, please report on
http://www.github.com/jeffbdn/tools-bundle/issues
