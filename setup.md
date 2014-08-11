# SlimVC Setup Documentation

## Installation
- clone this repository into your `themes/` folder.
- install composer (https://getcomposer.org/download/) 
- run `composer install` inside `themes/<your-theme>/app/` directory.

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

After that are you ready to create / modify the files in `app/Config` directory to setup your post types, taxonomies, templates, imagesizes etc.

Next step: [Configuration](https://github.com/moolen/SlimVC-documentation/tree/master/configuration.md).