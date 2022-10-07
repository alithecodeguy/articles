<h1 align="center">Making API Requests</h1>

Now that our app has a GitHub access token for the user, we can use it to make API requests. Let’s add a new section to our application that will run when the user clicks the “View Repos” link we created earlier.

Remember the `apiRequest` function we set up earlier? That’s where the access token is included in the HTTP request. The request this code will make will include the access token in the HTTP `Authorization` header, as illustrated in the example below.

```
GET /user/repos?sort=created&direction=desc HTTP/1.1
Host: api.github.com
Accept: application/vnd.github.v3+json
User-Agent: https://example-app.com/
Authorization: Bearer e2f8c8e136c73b1e909bb1021b3b4c29
```

The code below will take the access token and use it in a request to list the user’s repositories. It will then output a list of repositories and link to each one.

```
if(isset($_GET['action']) && $_GET['action'] == 'repos') {
  // Find all repos created by the authenticated user
  $repos = apiRequest($apiURLBase.'user/repos?'.http_build_query([
    'sort' => 'created', 'direction' => 'desc'
  ]));

  echo '<ul>';
  foreach($repos as $repo)
    echo '<li><a href="' . $repo['html_url'] . '">'
      . $repo['name'] . '</a></li>';
  echo '</ul>';
}
```

That’s it! You can now use the access token to make API requests to any of the API endpoints on GitHub! You can see the full documentation of GitHub’s API at https://developer.github.com/v3/.

Download the Sample Code
You can download the complete sample code used in this example from GitHub at https://github.com/aaronpk/sample-oauth2-client.
