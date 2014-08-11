# Configuration

You configure your Application inside `app/Config` directory, this includes the registration of custom post types, custom taxonomies, menus, sidebars, image size definition post-templates and a application configuration.

The following files inside `app/Config` are parsed:

- application.php
- images.php
- menus.php
- postTypes.php
- routes.php
- sidebars.php
- taxonomies.php
- templates.php

The setup is pretty straight-forward. Just look at the following examples:

## Image Sizes
Image sizes are defined as associative array. The `key` is the image size name and the `value` an array containing x, y, and crop-mode (in order!).
```PHP
// images.php
return array(
	'my_size' => array(200, 256),
	// size-name                x    y  crop-mode
	'my_size_cropped' => array(200, 256, true),
);
```

## Menu Configuration
Menus are defined as associative array. `key` is the slug, `value` the pretty name.
```PHP
// menus.php
return array(
	// slug         => Pretty name   
	'header-nav'	=> 'Header navigation',
	'footer-nav'	=> 'Footer navigation'
);
```

## Post Types

Post Types are defined as associative array; The `key` is the post-type-slug and the `value` is an array like the `$args` array for [`register_post_type`](http://codex.wordpress.org/Function_Reference/register_post_type).
```PHP
// postType.php
return array(
	// CPT name -> array
	'books' => array(
		'public'             => true,
		'publicly_queryable' => true,
		'show_ui'            => true,
		'show_in_menu'       => true,
		'query_var'          => true,
		'rewrite'            => array( 'slug' => 'book' ),
		'capability_type'    => 'post',
		'has_archive'        => true,
		'hierarchical'       => false,
		'menu_position'      => null,
		'supports'           => array( 'title', 'editor', 'author', 'thumbnail' ),
		'labels'             => array(
			'name'               => _x( 'Books', 'post type general name', 'my-app' ),
			'singular_name'      => _x( 'Book', 'post type singular name', 'my-app' ),
			'menu_name'          => _x( 'Books', 'admin menu', 'my-app' ),
			'name_admin_bar'     => _x( 'Book', 'add new on admin bar', 'my-app' ),
			'add_new'            => _x( 'Add New', 'book', 'my-app' ),
			'add_new_item'       => __( 'Add New Book', 'my-app' ),
			'new_item'           => __( 'New Book', 'my-app' ),
			'edit_item'          => __( 'Edit Book', 'my-app' ),
			'view_item'          => __( 'View Book', 'my-app' ),
			'all_items'          => __( 'All Books', 'my-app' ),
			'search_items'       => __( 'Search Books', 'my-app' ),
			'parent_item_colon'  => __( 'Parent Books:', 'my-app' ),
			'not_found'          => __( 'No books found.', 'my-app' ),
			'not_found_in_trash' => __( 'No books found in Trash.', 'my-app' )
		),
	)
);
```

## Sidebars
Sidebars are configured as array-of-arrays:
Each inner Array contains the arguments for [`register_sidebar`](http://codex.wordpress.org/Function_Reference/register_sidebar).
```PHP
// sidebars.php
return array(
	array(
		'name'			=> 'Blog',
		'id'			=> 'blog-sidebar',
		'description'	=> 'Blog sidebar.',
		'before_widget'	=> '<div class="sidebar-widget"><div class="sidebar-widget_content">',
		'after_widget'	=> '</div></div>',
		'before_title'	=> '<h4>',
		'after_title'	=> '</h4>'
	)
);
```

## Taxonomies

Taxonomies are defined as associative array. `key` is the taxonomy's slug, `value` an assoc. array: 
`postType` defines the scope of the taxonomy (the post-type) and `args` is the `$args` array of [`register_taxonomy`](http://codex.wordpress.org/Function_Reference/register_taxonomy).
```PHP
// taxonomies.php
return array(
	'genre' => array(
		// string or array of post type(s)
		'postType' => 'books',
		'args' => array(
			'labels'            => array(
				'name'              => _x( 'Genres', 'taxonomy general name' ),
				'singular_name'     => _x( 'Genre', 'taxonomy singular name' ),
				'search_items'      => __( 'Search Genres' ),
				'all_items'         => __( 'All Genres' ),
				'parent_item'       => __( 'Parent Genre' ),
				'parent_item_colon' => __( 'Parent Genre:' ),
				'edit_item'         => __( 'Edit Genre' ),
				'update_item'       => __( 'Update Genre' ),
				'add_new_item'      => __( 'Add New Genre' ),
				'new_item_name'     => __( 'New Genre Name' ),
				'menu_name'         => __( 'Genre' ),
			),
			'hierarchical'      => true,
			'show_ui'           => true,
			'show_admin_column' => true,
			'query_var'         => true,
			'rewrite'           => array( 'slug' => 'genre' ),
		)
	)
);
```

## Page-Templates
Page templates just like menus defined as associative array. `key` is slug, `value` is pretty name.
```PHP
// templates.php
return array(
	// slug       ->  pretty name
	'my-template' => 'Fresh Example Template'
);
``` 

## Application Configuration
Here you can define application-wide settings for SlimVC. The following values are the default values. You can override the settings in your `application.php` config file. A detailed explanation is below. 
```PHP
// application.php
return array(
	'debug' => true,
	'namespace.controller' => '\\App\\Conrollers\\',
	'method.seperator' => '::',
	'log.level' => 8,
	'log.enabled' => true,
	'templates.path' => dirname(__FILE__) . '/../Views'
);
```

### Application Configuration Options

All of the following options are passed into the `Slim` instance.

##### `debug` (boolean)
En-/Disable debugging mode. En-/Disables caching (For Twig & config files).

##### `namespace.controller` (string)

Sets the Namespace for the Controllers. default is `\App\Controllers`

##### `method.seperator` (string)

What method seperator you would like to use whe configuring routes. default is `::`. 

##### `log.level` (int, 1-8)
Set the log level from 1 to 8. 8 is verbosest.

##### `log.enabled` (boolean)
En-/Disable the logger.

##### `templates.path`
Defines the directory that holds the Twig templates.

## Advanced Customfields
With version 5 ACF field definitions are saved automatically as json inside the theme directory. With SlimVC those files are saved within `app/Config/acf`.