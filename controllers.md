# Controller

How you implement the Controller is up to you. This is a demonstration of how it could be implemented.
The constructor gets 2 arguments: 

- 1. [SlimVC Singleton](https://github.com/moolen/SlimVC-documentation/tree/master/slimvc.md) instance
- 2. $params array containing optional [route parameter](https://github.com/moolen/SlimVC-documentation/tree/master/routing.md)

The Callback or constructor recieves 2 Arguments: `$App` & `$params`.
First is a instance of the [SlimVC Singleton](https://github.com/moolen/SlimVC-documentation/tree/master/slimvc.md), second are the optional routing parameters. For example, if you create a explicit route `/foo/:param1/:param2` and `/foo/bar/baz` is requested $params would be `array('bar', 'baz')`.

This is a example of a implementation

```PHP
namespace App\Controllers;

use \App\Models\PostsModel as PostsModel;

class PostsController{
	public function __construct( $App, $params ){
		$this->App = $App;
		$this->params = $params;
		$this->PostsModel = new PostsModel($App->post);
		$this->render();
	}

	public function render(){
		// get posts from the model
		$posts = $this->PostsModel::getSomePosts();

		// render the view and inject 'posts' and 'title' into the TWIG view.
		return $this->App->render('posts.html',
			array(
				'posts' => $posts,
				'title' => 'my-app'
			)
		);
	}
}

```

## Recommendation

When defining your routes group them in a meaningful way. For example, if you have your custom post type `books` and you want a archive and single-item-page you can use a single Controller. Setup the shared Model in the constructor and let the method render the specific template.

Example code is in the [routing documentation](https://github.com/moolen/SlimVC-documentation/tree/master/routing.md).