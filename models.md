# SlimVC Models Documentation

## PostModel 

SlimVC comes with a abstraction of the WP_Post Object (`\App\Lib\SlimVC\PostModel`).
This provides you a neat interface to the WP_Post Object:

- getTitle($strlen, $raw, $closure, $ellipsis)
- getPost()
- getContent($strlen, $raw, $ellipsis)
- getFeaturedImageUrl()
- getCustomField($name)
- getDate($format)
- getSubPosts( $args )
- hasSubPosts()
- isChildOf( $parent )
- isInCategory($slug)

(More documentation coming soon)


## Models
It is totally up to you how you implement a Model. This is a example of how you could using Iterator Aggregate.

```PHP

namespace App\Models;

use \ArrayIterator;
use \IteratorAggregate;
use \App\Lib\SlimVC\PostModel;

// implement IteratorAggregate to iterate conveniently
// over the model inside the twig-view
class BooksModel implements \IteratorAggregate{

	protected $posts;

	public function __construct(){
		$this->posts = $this->getPosts();
	}

	public function getPosts(){

		// fetch all posts
		$posts = \get_posts(array(
			'post_type' => 'books',
			'post_status' => 'publish',
			'posts_per_page' => -1
		));

		// return new PostModel Instance per Post
		return array_map(function( $post ){
			return new PostModel( $post );
		}, $posts);
	}

	// Implement IteratorAggregate
	public function getIterator(){
		return new \ArrayIterator( $this->posts );
	}

}

```