# wp_help_sheet
Wordpress help sheet etc.
### ACF code examples
```
https://www.advancedcustomfields.com/resources/code-examples/
```

### Get  data from  table 

```
global $wpdb;
$user = $wpdb->get_results( "SELECT * FROM table_name" );

```

### Get page 

```
$post  = get_page($page_id);
$post->post_title;
```



### Get youtube title

```
explode('</title>', explode('<title>', file_get_contents("https://www.youtube.com/watch?v=". $video_id .""))[1])[0];
```


### Displaying parmalink for current post

```
the_permalink()
```

### Get Data by  date from database wp_query
```
<?php
    $query = new wp_query( 'post_status=any&order=asc' );

    $posts_by_day = array_reduce( $query->posts, function( $r, $v ) {
        $r[ date( 'Y-m-d', strtotime( $v->post_date ) ) ][] = $v;
        return $r;
    });
?>

<?php if ( $posts_by_day ) : ?>
<div class="day-posts">
	<?php foreach( $posts_by_day as $day => $day_posts ) : ?>
		<div class="day">
		    <div class="title"><?php echo date( 'l jS F Y', strtotime( $day ) ); ?></div>
		    <div class="posts">
		    <?php foreach( $day_posts as $post ) : setup_postdata( $post ); ?>
		        <div class="post">
		            <div class="title"><?php the_title(); ?></div>
		            <div class="content"><?php the_content(); ?></div>
		        </div>
		    <?php endforeach; ?>
		    </div>
		</div>
	<?php endforeach; wp_reset_postdata(); ?>
	</div>
<?php endif; ?>
```
