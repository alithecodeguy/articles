<h1 align="center">Setting up the Environment</h1>

This example code is written in PHP with no external packages required and no framework needed. Hopefully this makes it easy to translate to other languages if desired. To follow along with this example code, you can place it all in a single PHP file.

Create a new folder and create an empty file in that folder called `index.php`. From the command line, run php -S `localhost:8000` from inside that folder, and you’ll be able to visit http://localhost:8000 in your browser to run your code. All the code in the examples below should be added to this `index.php` file.

Let’s set up a few variables we’ll need for the OAuth process, adding the client ID and secret we got from Google when we created the application.

```
// Fill these out with the values you got from Google
$googleClientID = '';
$googleClientSecret = '';

// This is the URL we'll send the user to first
// to get their authorization
$authorizeURL = 'https://accounts.google.com/o/oauth2/v2/auth';

// This is Google's OpenID Connect token endpoint
$tokenURL = 'https://www.googleapis.com/oauth2/v4/token';

// The URL for this script, used as the redirect URL
$baseURL = 'https://' . $_SERVER['SERVER_NAME']
    . $_SERVER['PHP_SELF'];

// Start a session so we have a place
// to store things between redirects
session_start();
```

With those variables defined, and the session started, let’s set up the logged in and logged out pages. We’ll show a super simple page that just indicates whether the user is logged in or not, and has a link to log in or log out.

```
// If there is a user ID in the session
// the user is already logged in
if(!isset($_GET['action'])) {
  if(!empty($_SESSION['user_id'])) {
    echo '<h3>Logged In</h3>';
    echo '<p>User ID: '.$_SESSION['user_id'].'</p>';
    echo '<p>Email: '.$_SESSION['email'].'</p>';
    echo '<p><a href="?action=logout">Log Out</a></p>';

    // Fetch user info from Google's userinfo endpoint
    echo '<h3>User Info</h3>';
    echo '<pre>';
    $ch = curl_init('https://www.googleapis.com/oauth2/v3/userinfo');
    curl_setopt($ch, CURLOPT_HTTPHEADER, [
      'Authorization: Bearer '.$_SESSION['access_token']
    ]);
    curl_exec($ch);
    echo '</pre>';

  } else {
    echo '<h3>Not logged in</h3>';
    echo '<p><a href="?action=login">Log In</a></p>';
  }
  die();
}
```

The logged-out view contains a link to our login URL which starts the flow.

[Previous](https://github.com/alithecodeguy/articles/blob/main/OAuth/OAuth%202.0%20Simplified/03%20Signing%20in%20with%20Google/01%20Create%20an%20Application/CreateAnApplication_en.md "Previous")
/
[Next](https://github.com/alithecodeguy/articles/blob/main/OAuth/OAuth%202.0%20Simplified/03%20Signing%20in%20with%20Google/03%20Authorization%20Request/AuthorizationRequest_en.md "Next")
