# Setup

## Installation
clone this repository into your `themes/` folder.
install composer (https://getcomposer.org/download/) and run `composer install` inside `themes/<your-theme>/app/` directory.
Be sure to create a .htaccess file (e.g. by setting permalinks in the wordpress admin area)

## Configuration
Be sure to instantiate the SlimVC Singleton in your functions.php. This will parse the Config/ dir, setup the wp-core-hooks, instantiate the Router.

```PHP
// @functions.php
require 'app/vendor/autoload.php';
$App = App\Lib\SlimVC\SlimVC::getInstance();
```
After that are you ready to create / modify the files in `app/Config` directory to setup your CPT, CT, templates, imagesizes etc.
Take a look @configuration.md