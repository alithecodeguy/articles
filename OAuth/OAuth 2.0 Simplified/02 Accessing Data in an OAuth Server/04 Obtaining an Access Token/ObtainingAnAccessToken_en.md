<h1 align="center">Obtaining an Access Token</h1>

When the user is redirected back to our app, there will be a code and `state` parameter in the query string. The `state` parameter will be the same as the one we set in the initial authorization request, and is meant for our app to check that it matches before continuing. This helps our app avoid being tricked into sending an attacker’s authorization code to GitHub, as well as prevents CSRF attacks.

```
// When GitHub redirects the user back here,
// there will be a "code" and "state" parameter in the query string
if(isset($_GET['code'])) {
  // Verify the state matches our stored state
  if(!isset($_GET['state'])
    || $_SESSION['state'] != $_GET['state']) {

    header('Location: ' . $baseURL . '?error=invalid_state');
    die();
  }

  // Exchange the auth code for an access token
  $token = apiRequest($tokenURL, array(
    'grant_type' => 'authorization_code',
    'client_id' => $githubClientID,
    'client_secret' => $githubClientSecret,
    'redirect_uri' => $baseURL,
    'code' => $_GET['code']
  ));
  $_SESSION['access_token'] = $token['access_token'];

  header('Location: ' . $baseURL);
  die();
}
```

Here we are sending a request to GitHub’s token endpoint to exchange the authorization code for an access token. The request contains our public client ID as well as the private client secret. We also send the same redirect URL as before along with the authorization code.

If everything checks out, GitHub generates an access token and returns it in the response. We store the access token in the session and redirect to the home page, and the user is logged in.

The response from GitHub will look like the below.

```
{
  "access_token": "e2f8c8e136c73b1e909bb1021b3b4c29",
  "token_type": "Bearer",
  "scope": "public_repo,user"
}
```

Our code has extracted the access token and saved it in the session. The next time you visit the page, it recognizes that there’s already an access token and shows the logged-in view we created earlier.

Note: We have not included any error handling code in this example for simplicity’s sake. In reality, you’d check for errors returned from GitHub and display an appropriate message to the user.

[Previous](https://github.com/alithecodeguy/articles/blob/main/OAuth/OAuth%202.0%20Simplified/02%20Accessing%20Data%20in%20an%20OAuth%20Server/03%20Authorization%20Request/AuthorizationRequest_en.md "Previous")
/
[Next](https://github.com/alithecodeguy/articles/blob/main/OAuth/OAuth%202.0%20Simplified/02%20Accessing%20Data%20in%20an%20OAuth%20Server/05%20Making%20API%20Requests/MakingAPIRequests_en.md "Next")
