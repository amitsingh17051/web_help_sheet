# wp_help_sheet
Wordpress help sheet etc.

### Get  data from  table 

```
global $wpdb;
$user = $wpdb->get_results( "SELECT * FROM table_name" );

```

## Get page 

```
$post  = get_page($page_id);
$post->post_title;
```

