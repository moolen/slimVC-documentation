# Controller

How you implement the Controller is up to you. This is a demonstration of how it could be implemented.
The constructor always gets 2 arguments: 

- 1. SlimVC Singleton instance
- 2. $params array containing optional route parameter

with the $App instance you can render a view with:
`$App->render('path/to/view.ext', array('inject' => 'this variable into view'))`

```PHP
namespace App\Controllers;

use \App\Models\PostsModel as PostsModel;

class PostsController{
	public function __construct( $App, $params ){
		$this->App = $App;
		$this->params = $params;
		$this->PostsModel = new PostsModel();
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

Example code is @route documentation.