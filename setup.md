# SlimVC Setup Documentation

## Installation
clone the repository into your `themes/` folder.
```
git clone https://github.com/moolen/SlimVC.git
```
install composer (https://getcomposer.org/download/)
```
curl -sS https://getcomposer.org/installer | php
```

Install the dependencies inside `themes/<your-theme>/app/` directory.
```
composer install
```

Be sure to create a .htaccess file (e.g. by setting permalinks in the wordpress admin area).

## Configuration
Be sure to instantiate the SlimVC Singleton in your `functions.php`. This will parse the `app/Config/` directory, setup the wp-core-hooks and instantiates the Router.

```PHP
// @functions.php
// load composers' PSR-0 autoloader
require 'app/vendor/autoload.php';
// bootstrap SlimVC
$App = App\Lib\SlimVC\SlimVC::getInstance();
```

## Caching
Rendering Twig Templates is expensive, by default a cache directory is added in the theme's root folder. Be sure that the folder exists & is writable for your PHP/Apache user.
The Configuration files are cached (if available) with PHP-APC **ONLY with debug=FALSE** in your [application configuration](https://github.com/moolen/slimVC-documentation/blob/master/configuration.md#application-configuration).

## Initialization Process
![Initialization flowchart](https://docs.google.com/drawings/d/1XmPrCrD5kzfIJZX48SML6O9aq_JcLgjx2qUVW8gFdw4/pub?w=1087&h=1266)

After that are you ready to configure your Application: [Next Step: Configuration](https://github.com/moolen/SlimVC-documentation/tree/master/configuration.md).