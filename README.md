# WordPress Tips, Tricks and Hacks
Some of the most wanted WordPress tips, tricks, and hacks that will help you use WordPress.

### Get all post types in WordPress

```php
function demo_get_post_types( $args = [] ) {

	$post_type_args = [
		// Default is the value $public.
		'show_in_nav_menus' => true,
	];

	// Keep for backwards compatibility.
	if ( ! empty( $args['post_type'] ) ) {
		$post_type_args['name'] = $args['post_type'];
		unset( $args['post_type'] );
	}

	$post_type_args = wp_parse_args( $post_type_args, $args );

	$_post_types = get_post_types( $post_type_args, 'objects' );

	$post_types = [];

	foreach ( $_post_types as $post_type => $object ) {
		
		$post_types[ $post_type ] = $object->label;
	}

	return apply_filters( 'demo_get_public_post_types', $post_types );
}
```

### Avoid off spammers

```php
function preprocess_new_comment( $commentdata ) {
	if(!isset($_POST['comment_author'])) {
		die( 'Sorry comments are disabled in our demo site, thank you.' );
	}
	return $commentdata;
}
add_action( 'preprocess_comment', 'preprocess_new_comment' );
```

### Disable auto update

```php
add_filter( 'auto_update_plugin', '__return_false' );
```

```php
function remove_core_updates_wp(){
   global $wp_version;return(object) array('last_checked'=> time(),'version_checked'=> $wp_version,);
}
add_filter('pre_site_transient_update_core','remove_core_updates_wp');
add_filter('pre_site_transient_update_plugins','remove_core_updates_wp');
add_filter('pre_site_transient_update_themes','remove_core_updates_wp');
```


### Search and replace old urls to new urls in database

```php
UPDATE wp_options SET option_value = replace(option_value, 'https://old.example.com', 'https://new.example.com') WHERE option_name = 'home' OR option_name = 'siteurl';
UPDATE wp_posts SET guid = replace(guid, 'https://old.example.com','https://new.example.com');
UPDATE wp_posts SET post_content = replace(post_content, 'https://old.example.com','https://new.example.com');
UPDATE wp_termmeta SET meta_value = replace(meta_value, 'https://old.example.com','https://new.example.com');
UPDATE wp_postmeta SET meta_value = replace(meta_value,'https://old.example.com','https://new.example.com');
```

### Search and replace old urls to new urls in database for customize option

```php
function replce_oldurl_to_newurl_customizer() {
		
	$output = get_option('theme_mods_helloworld');
	$output = str_replace("https:\/\/old.example.com\/wp-content\/uploads\/", "https:\/\/new.example.com\/demo-images\/", json_encode($output));
	$output = str_replace("https:\/\/old.example.com\/", "https:\/\/new.example.com\/", $output);
	echo $output;
	die;
}
add_action( 'init', 'replce_oldurl_to_newurl_customizer' );
```

### To increase WordPress’ memory limit
To increase WordPress’ memory limit, you can define WP_MEMORY_LIMIT and WP_MAX_MEMORY_LIMIT in your wp-config.php file using the line below.

<code>define( 'WP_MEMORY_LIMIT', '512M' );</code>

<code>define( 'WP_MAX_MEMORY_LIMIT', '512M');</code>

### In addition, you can also disable this auto-save feature, and decide the maximum number of post revision.

<code>define( 'WP_POST_REVISIONS', false );</code>

<code>define( 'WP_POST_REVISIONS', 5 );</code>
