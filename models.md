# Models

It is totally up to you how you implement a Model. This is a example of how you could.

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

# PostModel 

SlimVC comes with a abstraction of the WP_Post Object (`\App\Lib\SlimVC\PostModel`).
This provides you a neat interface to the WP_Post Object:

- $PostModel->getTitle($strlen, $raw, $closure, $ellipsis)
- $PostModel->getPost()
- $PostModel->getContent($strlen, $raw, $ellipsis)
- $PostModel->getFeaturedImageUrl()
- $PostModel->getCustomField($name)
- $PostModel->getDate($format)
- $PostModel->getSubPosts( $args )
- $PostModel->hasSubPosts()
- $PostModel->isChildOf( $parent )
- $PostModel->isInCategory($slug)
- PostModel::get_a_Title($post, $strlen, $raw, $ellipsis)
- PostModel::get_a_CustomField($post, $name)
- PostModel::get_a_FeaturedImageUrl($post)
- PostModel::get_a_Date($post, $format)
- PostModel::get_the_CustomFields($post)
- PostModel::get_the_subPosts($post, $args)
- PostModel::get_a_PostBySlug($slug, $wpPostObj)
- PostModel::has_PostSubPosts($post)
- PostModel::is_PostChildOf($post, $parent)
- PostModel::is_PostInCategory($post, $slug)