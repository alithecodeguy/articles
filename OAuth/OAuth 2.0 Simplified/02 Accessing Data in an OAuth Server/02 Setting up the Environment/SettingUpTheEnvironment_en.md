<h1 align="center">Setting up the Environment</h1>

This example code is written in PHP with no external packages required and no framework needed. Hopefully this makes it easy to translate to other languages if desired. To follow along with this example code, you can place it all in a single PHP file.

Create a new folder and create an empty file in that folder called `index.php`. From the command line, run `php -S localhost:8000` from inside that folder, and you’ll be able to visit http://localhost:8000 in your browser to run your code. All the code in the examples below should be added to this `index.php` file.

To make things easier for us, let’s define a method, `apiRequest()` which is a simple wrapper around cURL. This function will include the `Accept` and `User-Agent `headers that GitHub’s API requires, and automatically decode the JSON response. If we have an access token in the session, it will send the proper OAuth header with the access token as well, in order to make authenticated requests.

```
function apiRequest($url, $post=FALSE, $headers=array()) {
  $ch = curl_init($url);
  curl_setopt($ch, CURLOPT_RETURNTRANSFER, TRUE);

  if($post)
    curl_setopt($ch, CURLOPT_POSTFIELDS, http_build_query($post));

  $headers = [
    'Accept: application/vnd.github.v3+json, application/json',
    'User-Agent: https://example-app.com/'
  ];

  if(isset($_SESSION['access_token']))
    $headers[] = 'Authorization: Bearer '.$_SESSION['access_token'];

  curl_setopt($ch, CURLOPT_HTTPHEADER, $headers);

  $response = curl_exec($ch);
  return json_decode($response, true);
}
```

Now let’s set up a few variables we’ll need for the OAuth process.

```
// Fill these out with the values from GitHub
$githubClientID = '';
$githubClientSecret = '';

// This is the URL we'll send the user to first
// to get their authorization
$authorizeURL = 'https://github.com/login/oauth/authorize';

// This is the endpoint we'll request an access token from
$tokenURL = 'https://github.com/login/oauth/access_token';

// This is the GitHub base URL for API requests
$apiURLBase = 'https://api.github.com/';

// The URL for this script, used as the redirect URL
$baseURL = 'https://' . $_SERVER['SERVER_NAME']
    . $_SERVER['PHP_SELF'];

// Start a session so we have a place to
// store things between redirects
session_start();
```

First, let’s set up the “logged-in” and “logged-out” views. This will show a simple message indicating whether the user is logged in or logged out.

```
// If there is an access token in the session
// the user is already logged in
if(!isset($_GET['action'])) {
  if(!empty($_SESSION['access_token'])) {
    echo '<h3>Logged In</h3>';
    echo '<p><a href="?action=repos">View Repos</a></p>';
    echo '<p><a href="?action=logout">Log Out</a></p>';
  } else {
    echo '<h3>Not logged in</h3>';
    echo '<p><a href="?action=login">Log In</a></p>';
  }
  die();
}
```

The logged-out view contains a link to our login URL which starts the OAuth process.

[Previous](https: "Previous")
/
[Next](https: "Next")
