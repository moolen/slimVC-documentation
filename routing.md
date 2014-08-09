# Routing

Two different Routing options are available, **explicit** routes and **conditional** routes.
In each route you map an incoming request to a controller.

## Explicit Routes

Explicit routes take precedence over conditional routes; You define a HTTP Method a path and map that to a controller.

the following HTTP Methods are supported:
- `GET`
- `POST`
- `PUT`
- `DELETE`
- `PATCH`

```PHP
// @app/Config/routes.php
return array(
	'explicit' => array(
		array(
			// default method is GET
			'method' => 'GET',
			'path' => '/my/path',
			'controller' => 'MyController::myMethod'
		),
		array(
			'method' => 'GET',
			// :title is a variable parameter
			// this can be anything
			'path' => '/books/:title',
			'controller' => 'BooksController'
		)
	),
	'conditional' => array(/****/)
);

```

## Conditional Routes

Conditonal routes are a little bit more complex; they use WP's conditional tags under the hood (keep that in mind). You define the conditions under what circumstances a controller is invoked, the configuration is pretty straight-forward. Lets take a example:

You want a custom post type ('books') archive mapped to 'MyController::archive' (archive is the method on MyController), then you go:

```PHP
// @app/Config/routes.php
return array(
	'explicit' => array(/****/),
	'conditional' => array(
		array(
			'post_type' => 'books',
			'archive' => true,
			'controller' => 'MyController::archive'
		)
	)

);
```

A little more complex situation:

```PHP
// @app/Config/routes.php
return array(

	'explicit' => array(

		array(
			'method'		=> 'GET',
			'path'			=> '/my-books/(:title?)',
			'controller'	=> 'BooksController'
		),

		array(
			// GET method is default
			'path'			=> '/baz/:bee',
			'controller'	=> 'PostsController:foo'
		),

	),

	'conditional' => array(

		array(
			'page' => true,
			//'page_template' => 'my-template',
			'controller' => 'PageController'
		),

		array(
			'archive' => true,
			'post_type' => 'books',
			'controller' => 'BooksController:archive'
		),

		array(
			'single' => true,
			'post_type' => 'books',
			'controller' => 'BooksController:single'
		),

		array(
			'home' => true,
			'controller' => 'PostsController'
		),

		array(
			'404' => true,
			'controller' => 'NotFoundController'
		)

	)

);
```

## Constructor Arguments

The Callback or constructor recieves 2 Arguments: `$App` & `$params`.
First is a instance of the SlimVC Singleton, second are the optional routing parameters. For example, if you create a explicit route `/foo/:param1/:param2` and `/foo/bar/baz` is requested $params would be `array('bar', 'baz')`.

The documentation about the SlimVC Singleton is here.

## Routing recommendation

Consider the following route setup:
You have your custom post type 'books' and you want to render a archive and a single-item page. But you do not want to create a controller and a model for each Route. 
Instead, you can create a shared controller that sets up the shared model and the method aggregates the data from the model and sends it to the view.

In th application configuration you can set the `method.seperator`, default is `::`.

```PHP
// @app/Config/routes.php
return array(
	'explicit' => array(/***/),
	'conditional' => array(
		array(
			'archive' => true,
			'post_type' => 'books',
			'controller' => 'BooksController::archive'
		),
		array(
			'single' => true,
			'post_type' => 'books',
			'controller' => 'BooksController::single'
		),
	)
);
```

The Router will instantiate the BooksController and then calls the defined method (`single`, or `archive`).

```PHP
// @app/Controllers/BooksController.php

class BooksController{
	// constructor sets up the model and $App object
	public function __construct($App, $params){
		$this->App = $App,
		$this->params = $params;
		$this->model = new \App\Models\BooksModel($this->App->post);
	}
	// renders single-page
	public function single(){
		$post = $this->model->getSingle();
		$this->App->render('books-single.html', array('post' => $post));
	}
	// renders archive page
	public function archive(){
		$posts = $this->model->getArchive();
		$this->App->render('book-archive.html', array('posts' => $posts));
	}
}

```
