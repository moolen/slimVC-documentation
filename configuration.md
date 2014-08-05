# Configuration

You configure your App in app/Config directory, this includes the registration of custom post types, custom taxonomies, menus, sidebars, image size definition post-templates and a application configuration.

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

```PHP
// images.php
return array(
	'my_size' => array(200, 256),
	// size-name                x    y  crop-mode
	'my_size_cropped' => array(200, 256, true),
);
```

## Menu Configuration

```PHP
// menus.php
return array(
	// slug         => Pretty name   
	'header-nav'	=> 'Header navigation',
	'footer-nav'	=> 'Footer navigation'
);
```

## Post Types

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

```PHP
// templates.php
return array(
	// slug       ->  pretty name
	'my-template' => 'Fresh Example Template'
);
``` 

## Application Configuration

```PHP
// application.php
return array(
	'debug' => true,
	'namespace.controller' => '\\App\\Conrollers\\',
	'method.seperator' => ':',
	'slim' => array(
		// env vars
		'mode' => 'development',
		'log.enabled' => true,
		'log.writer' => new \App\Lib\SlimVC\Logger(),
		'log.level' => \Slim\Log::DEBUG,

		// view & templating
		'view' => new \Slim\Views\Twig(),
		'templates.path' => dirname(__FILE__) . '/../Views',
	)
	
);
```

## Advanced Customfields
With version 5 ACF field definitions are saved automatically as json inside the theme directory. With SlimVC those files are saved within `app/Config/acf`.