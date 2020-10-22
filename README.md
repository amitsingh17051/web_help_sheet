# wp_help_sheet
Wordpress help sheet etc.

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
