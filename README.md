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

### WP Meta Query

```
$user_args = array(
    'orderby'=>'display_name',
    'order'=>'ASC', 
    'meta_query' => array(
        'relation' => 'OR' ,
        array(
            'key' => 'office' ,
            'value' => $userOfficeName,
            'compare' => '=',
        )
    )
);
$uq = new WP_User_Query($user_args);
$users_list = $uq->get_results();
```


### WP AJax Request

```
// In js file
jquery.ajax({
method:
data: {
'action': 'action_name',

}
});

// In php file
function action_func(){
// Code
wp_die();
}
add_action('wp_ajax_action_name', 'action_func');
add_action('wp_ajax_nopriv_action_name', 'action_func');
```

### Set Transiet inn WP
```
// check if this transiet is available
$responseBody= get_transient('artist_products_'.$_POST['artistid']);

if (!$responseBody){
	// if not then call api and set transiate
	$apiURL = "https://www.handmadeinbritain.co.uk/wp-json/virtual-fairs/artist-profile/56124/".$_POST['artistid'];

	$response = wp_remote_get( $apiURL, array('timeout'=>120));

	$responseBody = wp_remote_retrieve_body( $response );

	set_transient('artist_products_'.$_POST['artistid'], $responseBody, 15600);

}
```

### Filter Listbox With ids, class and search input in jquery
```

$('input[name="search"]').on("keyup", function() {
   rq_filter_option();
});
$('select[name="employee"]').on("change", function() {
    rq_filter_option();
});
$('select[name="projectname"]').on("change", function() {
   rq_filter_option();
});

function rq_filter_option( selector, searchIn ){
    var search = {};
    $(selector).each(function(i, el){
        $(document).on('keyup change', el, function(){
            search[i]['search'] = $(this).val();
        });
    });

	var search = $('input[name="search"]').val();
	var project = $('select[name="projectname"]').val();
	var user = $('select[name="employee"]').val();
    $('.password-manager-list')
    .find('.credentials')
    .hide()
    .filter(function() {
      var okproject = true;
      var okuser = true;
      var oksearch = true;

      if (project !== "all") {
        okproject = $(this).hasClass('credential-project-'+project);
        console.log(okproject);
      }
      if (user !== "all") {
        okuser =$(this).hasClass('credential-user-'+user);

      }
      if (search !== '') {
        oksearch = $(this).text().toLowerCase().indexOf(search) > -1;
      }
      //only fade a room if it satisfies all four conditions
      return okproject && okuser && oksearch;
    }).fadeIn('fast');
}


```
