<h1 align="center">Verifying the User Info</h1>

Normally, it’s critical that you validate an ID token before trusting any of the information inside it. This is because in other OpenID Connect flows your app will get an ID token over an untrusted channel such as a browser redirect.

In this case, you got the ID token from an HTTPS connection to Google using the client secret to authenticate the request, so you can be confident that the ID token you obtained did in fact come from the service and not an attacker. With this in mind, and I know it seems unsafe at first, it’s okay to decode the ID token without validating it. Even Google says so. https://developers.google.com/identity/protocols/OpenIDConnect#obtainuserinfo.

Take a look at the JWT above. It’s made up of three parts, each separated by a period. We can split the string on the dots, and take the middle piece. The middle piece is a base64-encoded JSON string containing the data about the user. Below is an example of the data in the JWT.

```
{
  "azp": "272196069173.apps.googleusercontent.com",
  "aud": "272196069173.apps.googleusercontent.com",
  "sub": "110248495921238986420",
  "hd": "okta.com",
  "email": "aaron.parecki@okta.com",
  "email_verified": true,
  "at_hash": "0bzSP5g7IfV3HXoLwYS3Lg",
  "exp": 1524601669,
  "iss": "https://accounts.google.com",
  "iat": 1524598069
}
```

All we really care about for this demo are the two properties sub and `email`. The sub (subject) property contains the unique user identifier of the user who signed in. We’ll extract that and store it in the session, which will indicate to our app that the user is signed in.

We’ll also store the ID token and access token in the session so we can use them later, to show an alternative way of getting the user info.

```
  // ... continuing from the previous code sample, insert this

  // Split the JWT string into three parts
  $jwt = explode('.', $data['id_token']);

  // Extract the middle part, base64 decode, then json_decode it
  $userinfo = json_decode(base64_decode($jwt[1]), true);

  $_SESSION['user_id'] = $userinfo['sub'];
  $_SESSION['email'] = $userinfo['email'];

  // While we're at it, let's store the access token and id token
  // so we can use them later
  $_SESSION['access_token'] = $data['access_token'];
  $_SESSION['id_token'] = $data['id_token'];

  header('Location: ' . $baseURL);
  die();
}
```

Now you’ll be redirected back to the app’s home page, where we’ll show you the user ID and email using the code we created at the beginning.

```
echo '<p>User ID: '.$_SESSION['user_id'].'</p>';
echo '<p>Email: '.$_SESSION['email'].'</p>';
```

## Using the ID Token to Retrieve User Info

Google provides an additional API endpoint, called the tokeninfo endpoint, which you can use to look up the ID token details instead of parsing it yourself. This is not recommended for production applications, as it requires an additional HTTP round trip, but can be useful for testing and troubleshooting.

Google’s tokeninfo endpoint is at https://www.googleapis.com/oauth2/v3/tokeninfo, as found in their OpenID Connect discovery document at https://accounts.google.com/.well-known/openid-configuration. To look up the info for the ID token we received, make a GET request to the tokeninfo endpoint with the ID token in the query string.

https://www.googleapis.com/oauth2/v3/tokeninfo?id_token=eyJ...
The response will be a JSON object with a similar list of properties that were included in the JWT itself.

```
{
 "azp": "272196069173.apps.googleusercontent.com",
 "aud": "272196069173.apps.googleusercontent.com",
 "sub": "110248495921238986420",
 "hd": "okta.com",
 "email": "aaron.parecki@okta.com",
 "email_verified": "true",
 "at_hash": "NUuq_yggZYi_2-13hJSOXw",
 "exp": "1524681857",
 "iss": "https://accounts.google.com",
 "iat": "1524678257",
 "alg": "RS256",
 "kid": "affc62907a446182adc1fa4e81fdba6310dce63f"
}
```

## Using the Access Token to Retrieve User Info

As mentioned before, many OAuth 2.0 services also provide an endpoint to retrieve the user info of the user who logged in. This is part of the OpenID Connect standard, and the endpoint will be part of the service’s OpenID Connect Discovery Document.

Google’s userinfo endpoint is https://www.googleapis.com/oauth2/v3/userinfo. In this case, you use the access token rather than the ID token to look up the user info. Make a GET request to that endpoint and pass the access token in the HTTP `Authorization` header like you normally would when making an OAuth 2.0 API request.

```
GET /oauth2/v3/userinfo
Host: www.googleapis.com
Authorization: Bearer ya29.Gl-oBRPLiI9IrSRA70...
```

The response will be a JSON object with several properties about the user. The response will always include the `sub` key, which is the unique identifier for the user. Google also returns the user’s profile information such as name (first and last), profile photo URL, gender, locale, profile URL, and email. The server can also add its own claims, such as Google’s hd showing the “hosted domain” of the account when using a G Suite account.

```
{
 "sub": "110248495921238986420",
 "name": "Aaron Parecki",
 "given_name": "Aaron",
 "family_name": "Parecki",
 "picture": "https://lh4.googleusercontent.com/-kw-iMgD
   _j34/AAAAAAAAAAI/AAAAAAAAAAc/P1YY91tzesU/photo.jpg",
 "email": "aaron.parecki@okta.com",
 "email_verified": true,
 "locale": "en",
 "hd": "okta.com"
}
```

## Download the Sample Code

You can download the complete sample code used in this example from GitHub at https://github.com/aaronpk/sample-oauth2-client.

You’ve seen three different ways to get the user’s profile info after the user signs in. So which one should you use and when?

For performance-sensitive applications where you might be reading ID tokens on every request or using them to maintain a session, you should definitely validate the ID token locally rather than making a network request. Google’s API docs provide a good guide on the details of validating ID tokens offline.

If all you’re doing is trying to find the user’s name and email after they sign in, then extracting the data from the ID token and storing it in your application session is the easiest and most straightforward option.

[Previous](https://github.com/alithecodeguy/articles/blob/main/OAuth/OAuth%202.0%20Simplified/03%20Signing%20in%20with%20Google/04%20Getting%20an%20ID%20Token/GettingAnIDToken_en.md "Previous")
/
[Next](https://github.com/alithecodeguy/articles/blob/main/OAuth/OAuth%202.0%20Simplified/04%20Server-Side%20Apps/Server-SideApps_en.md "Next")
