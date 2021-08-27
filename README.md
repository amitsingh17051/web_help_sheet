# help_sheet
Wordpress help sheet etc.

### Google My Bussiness
```
https://github.com/spotonlive/php-google-my-business
```

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
### Copy input text with or without password type

```
$( document ).on( 'click', '.copy-text', function(e){
    e.preventDefault();
    var $this = $(this).closest('.form-group').find('input');
    var copytarget = $this.prop("defaultValue");
    var $tempInput =  $("<textarea>");
    $("body").append($tempInput);
    $tempInput.val(copytarget).select();
    document.execCommand("copy");
    $tempInput.remove();
});

```
### Convert to variable Ajax serialize data 

```
parse_str($_POST['serialize'], $whatever);
extract($whatever);
```


### Abort Last ajax call

```
if(abortAjax){
	abortAjax.abort();
}
abortAjax = $.ajax({
	method: 'post',
	url: themeUrl + "/getsavedcredentials.php",
	data : {"id":savedvalue},
	success:function(res){
	}
});
```

### Best javascript structure for use in projects

```
var taskManager = {
    init: function() {

        // run on init, need to bind everything
        $(function(){

            $('.project-list-item').on('click', function() {
                $('.project-list-item').removeClass('active');
                $(this).addClass('active');

                $('#project_id_input').val( $(this).data('project_id') );

                taskManger.taskList.refresh();
            });

        });

        $(window).on('load', function(){

        });

    },
    taskList: {
        get: function() {

        },

    },
    taskDetail: {
        get: function( task_id ) {

        },
        save: function () {

        }
    }

}
```

### Get thumbnail image from video

```
public function getVideoThumbnail($videourl) {

	$currentTimestamp = new DateTime('now', new DateTimeZone('Asia/Kolkata'));
	$videourlarr = explode( '/', $videourl );
	$video_path = escapeshellarg(str_replace( get_stylesheet_directory_uri(), get_stylesheet_directory() , $videourl ));
	$video_name = end( $videourlarr );
	$upload_dir = wp_upload_dir(); 
	$image =  escapeshellarg($upload_dir['path'] . '/' . $video_name . '-' . $currentTimestamp->getTimestamp() . '-thumbnail.jpg');
	$videoThumbnailUrl =  $upload_dir['url'] . '/' . $video_name . '-' . $currentTimestamp->getTimestamp() .'-thumbnail.jpg';
	// scale=iw*sar:ih => To get video orginal size
	$thumbnail_generate_cmd = "ffmpeg -i $video_path -vframes 1 -an -vf scale=iw*sar:ih -ss 2 $image 2>&1";
	shell_exec($thumbnail_generate_cmd);
	return $videoThumbnailUrl;
	
}
```

### Adding menu item to wp dashbaord panel
```
// ibd-tracking-report => name of this page
public function register_tracking_report_menu() {
        $tracking_report = add_menu_page( __('Tracking Report') ,  __('Tracking Report'),  __('manage_options'),  'ibd-tracking-report',  array($this, 'display_tracking_report' ), 'dashicons-chart-bar',  20 );
	// loading script for only this tracking page
        add_action('load-'. $tracking_report, array($this,  'load_tracking_report_scripts') );
}


```


### OTP entering js code
```
jQuery('.verify-phone-number-text ul').find('li input').each(function() {
	jQuery(this).attr('maxlength', 1);
	jQuery(this).on('keyup', function(e) {
		var parent = jQuery(jQuery(this).parent().parent());
		if(e.keyCode === 8 || e.keyCode === 37) {
			var prev = parent.find('input#' + jQuery(this).data('previous'));
			
			if(prev.length) {
				jQuery(prev).select();
			}
		} else if((e.keyCode >= 48 && e.keyCode <= 57) || (e.keyCode >= 65 && e.keyCode <= 90) || (e.keyCode >= 96 && e.keyCode <= 105) || e.keyCode === 39) {
			var next = parent.find('input#' + jQuery(this).data('next'));
			
			if(next.length) {
				jQuery(next).select();
			} else {
				if(parent.data('autosubmit')) {
					parent.submit();
				}
			}
		}
	});
});
```

### Google sign in 
```
<?php
// Enable error reporting
error_reporting(E_ALL);
ini_set('display_errors', 1);

$google_redirect_url = 'REDIRECT_URL';

//start session
session_start();

//include google api files
include_once 'google-api-php-client/src/Google/autoload.php';

// New Google client
$gClient = new Google_Client();
$gClient->setApplicationName('ApplicationName');
$gClient->setAuthConfigFile('client_secret.json');
$gClient->addScope(Google_Service_Oauth2::USERINFO_PROFILE);
$gClient->addScope(Google_Service_Oauth2::USERINFO_EMAIL);

// New Google Service
$google_oauthV2 = new Google_Service_Oauth2($gClient);

// LOGOUT?
if (isset($_REQUEST['logout'])) 
{
	unset($_SESSION["auto"]);
	unset($_SESSION['token']);
	$gClient->revokeToken();
	header('Location: ' . filter_var($google_redirect_url, FILTER_SANITIZE_URL)); //redirect user back to page
}

// GOOGLE CALLBACK?
if (isset($_GET['code'])) 
{
	$gClient->authenticate($_GET['code']);
	$_SESSION['token'] = $gClient->getAccessToken();
    header('Location: ' . filter_var($google_redirect_url, FILTER_SANITIZE_URL));
    return;
}

// PAGE RELOAD?
if (isset($_SESSION['token'])) 
{
    $gClient->setAccessToken($_SESSION['token']);
}

// Autologin?
if(isset($_GET["auto"]))
{
	$_SESSION['auto'] = $_GET["auto"];
}

// LOGGED IN?
if ($gClient->getAccessToken()) // Sign in
{
	//For logged in user, get details from google using access token
	try {
		$user = $google_oauthV2->userinfo->get();
		$user_id              = $user['id'];
		$user_name            = filter_var($user['givenName'], FILTER_SANITIZE_SPECIAL_CHARS);
		$email                = filter_var($user['email'], FILTER_SANITIZE_EMAIL);
		$gender               = filter_var($user['gender'], FILTER_SANITIZE_SPECIAL_CHARS);
		$profile_url          = filter_var($user['link'], FILTER_VALIDATE_URL);
		$profile_image_url    = filter_var($user['picture'], FILTER_VALIDATE_URL);
		$personMarkup         = "$email<div><img src='$profile_image_url?sz=50'></div>";
		$_SESSION['token']    = $gClient->getAccessToken();
		
		// Show user
		echo '<br /><a href="'.$profile_url.'" target="_blank"><img src="'.$profile_image_url.'?sz=100" /></a>';
		echo '<br /><a class="logout" href="?logout=1">Logout</a>';
		
		$boolarray = Array(false => 'false', true => 'true');
		echo '<p>Was automatical login? '.$boolarray[isset($_SESSION["auto"])].'</p>';
		
		//list all user details
		echo '<pre>'; 
		print_r($user);
		echo '</pre>';  
	} catch (Exception $e) {
		// The user revoke the permission for this App! Therefore reset session token	
		unset($_SESSION["auto"]);
		unset($_SESSION['token']);
		header('Location: ' . filter_var($google_redirect_url, FILTER_SANITIZE_URL));
	}
}
else // Sign up
{
    //For Guest user, get google login url
    $authUrl = $gClient->createAuthUrl();
	
	// Fast access or manual login button?
	if(isset($_GET["auto"]))
	{
		header('Location: ' . filter_var($authUrl, FILTER_SANITIZE_URL));
	}
	else
	{
		echo '<p>Login?</p>';
		echo '<a class="login" href="'.$authUrl.'"><img src="images/google-login-button.png" /></a>';
	}
}
?>
```

