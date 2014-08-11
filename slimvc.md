# SlimVC Class Dicumentation

## Instantiation
The SlimVC Class is a Singleton. Get the instance by calling 

```PHP
$App = \App\Lib\SlimVC\SlimVC::getInstance();
```
The static `getInstance()` method actually returns a \Slim\Slim object which is the micro-framework behind SlimVC. See the documentation on \Slim\Slim [here](http://docs.slimframework.com).

### $App->request

Slim's Request object exposes the following public methods:

- getMethod()
- isGet()
- isPost()
- isPut()
- isDelete()
- isPatch()
- isHead()
- isOptions()
- isAjax()
- params()

More documentation about \Slim\Request [here](http://docs.slimframework.com).

### $App->response

If you do not want to use TWIG views (e.g. if you want to create a JSON response) you can use the Response Object directly.

```PHP
// @app/Controllers/JSONAPI.php
class JSONAPI{
	function __construct($App, $params){
		$this->App = $App;
		$this->params = $params;
	}

	public function getBooks(){
		$this->App->Response->headers->set('CONTENT_TYPE', 'application/json');
		$this->App->Response->setBody('{"foo":"bar"}');
		// done.
	}
}
```

Slim's Response object exposes the following public methods:

- headers->set()
- headers->get()
- getStatus()
- setStatus()
- getBody()
- setBody()
- write()
- redirect()

More documentation about \Slim\Response [here](http://docs.slimframework.com).

## $App->post

The post property is Wordpress' global $post object.

## Event API

The SlimVC Class exposes a eventdriven API to register callbacks to all Wordpress action hooks  `muplugins_loaded, plugins_loaded, setup_theme, after_setup_theme, init, wp_loaded, template_redirect`. These callbacks have to be registered inside the `functions.php` **otherwise the callbacks won't be called**. The Event API is available at the `$App->Event` object.

```PHP
// @functions.php
$App->Event->on('init', function(){
	// do init stuff
});

// also array form is supported
$App->Event->on('setup_theme', array('myClass', 'someMethod'));

$App->Event->on('wp_load', function(){
	// do wp_load stuff
});

```

Next step: [Controllers](https://github.com/moolen/SlimVC-documentation/tree/master/controllers.md).