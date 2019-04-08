=== WAJ Post Type & Meta Box Generators ===
Contributors: waughjai
Tags: link, post type, auto-generate
Requires at least: 4.9.8
Tested up to: 5.1.1
Stable tag: 1.0.0
Requires PHP: 7.0
License: GPLv2 or later
License URI: https://www.gnu.org/licenses/gpl-2.0.html

Simple plugin for easy generation o' new post types in theme code.

== Description ==

2 classes that you can use to easily create post types & meta boxes in theme code.

== Installation ==

1. Upload the plugin files to the `/wp-content/plugins/plugin-name` directory, or install the plugin through the WordPress plugins screen directly.
2. Activate the plugin through the 'Plugins' screen in WordPress
3. Add code in functions.php file or files loaded from it that create new instances o' classes WPPostType or WPMetaBox.

= Post Types =

* "singular_name": Public name for single entry o' post type. ( If post type is "Boxes", you would make this "Box". ) Defaults to group name.
* "supports": Editors this post type will show in the post type editor ( whether it will show the big body editor or the thumbnails link ). Defaults to showing just title & content editor. For the WordPress default, just set to false.
* "has_archive": A boolean that decides if post type has an archive page. Defaults to true.
* "public": A boolean that decides whether post type is public. Defaults to true.
* "meta_boxes": Array o' hash maps representing data for meta boxes that will automatically be setup. Hash maps should have values for "slug" & "name" keys to be valid. Other optional key values are the same as the optional extra arguments for meta boxes listed below. "post_type" value for each meta box will automatically be set to this post type 'less specifically set. ( This is mainly meant for backward compatibility with already-made meta boxes with slugs that don't fit pattern. 'Less a certain full slug is needed for meta boxes, I would recommend not bothering to o'erride this. )
* "rewrite": Sets archive permalink. Hash map with "slug" key set to value. Defaults to using slug.
* "meta_box_prefix": Overrides prefix that goes before slug o' each meta box slug when setting their full slug. Defaults to post type slug plus a hyphen separator.*
* "custom_toc": Array o' hash maps for columns to add to table view o' posts o' this type. Keys given values should be "slug" & "name". "Slug" should refer to the meta box slug & "name" should refer to the heading given to that column in the view.
* "unset_toc": Array o' default columns for admin list view to take off table.

= Meta Boxes =

Like with WPPostType class, you just need to create an instance o' this class before the admin loads & the constructor will automatically call all the WordPress functions needed to set a meta box up. Meta boxes added to Post Type constructor specified 'bove do not need to be added through this, as they are automatically added.

1st 2 mandatory arguments to constructor are the identifying slug & public title shown in editor. The optional 3rd argument is a hash map o' extra arguments.

* "post-type": Array o' post types this should show up in. Default is array with just "page".
* "input-type": String specifying type o' input HTML to form in the editor:
	* The default is "text".
	* 'Cept for the following exceptions, the any value will create an input tag with the type set to the given type. Thus, "number", "tel", "email", "password", & other HTML input types will work.
	* "textarea" will create a textarea tag.
	* "select" will create a select tag. Also requires optional argument "values", which should be an array o' hash maps with "id" & "name" key values. When used, the "id" value will be the actual value saved into the database, while the "name" value will show in the select box.
	* "day-of-the-week" will automatically create a select without needing the "values" argument. Values will be set to the 7 days o' week, whose IDs will be the #s 0 to 6 for Monday to Sunday.

== Example ==

= Post Type =

````
use WaughJ\WPPostType\WPPostType;

new WPPostType
(
	'news',
	'News',
	[
		'singular_name' => 'News Article',
		'supports' => [ 'title', 'editor', 'thumbnails' ],
		'meta_boxes' =>
		[
			[
				'slug' => 'url',
				'name' => 'URL'
			],
			[
				'slug' => 'order',
				'name' => 'Order',
				'input-type' => 'number'
			]
		],
		'unset_toc": [ 'date' ],
		'custom_toc':
		[
			[
				'slug' => 'order',
				'name' => 'Order'
			]
		]
	]
);
````

= Meta Box =

````
use WaughJ\WPMetaBox\WPMetaBox;

new WPMetaBox
(
	'color',
	'Color',
	[
		'post-type' => 'news-post',
		'input-type' => 'select',
		'values' =>
		[
			[ 'id' => '0', 'name' => 'Red' ],
			[ 'id' => '1', 'name' => 'Blue' ],
			[ 'id' => '2', 'name' => 'Green' ]
		]
	]
);
````

== Changelog ==

= 1.0 =
* Initial stable version.
