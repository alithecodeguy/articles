<h1 align="center">Authorization Code Grant</h1>

The authorization code is a temporary code that the client will exchange for an access token. The code itself is obtained from the authorization server where the user gets a chance to see what the information the client is requesting, and approve or deny the request.

The authorization code flow offers a few benefits over the other grant types. When the user authorizes the application, they are redirected back to the application with a temporary code in the URL. The application exchanges that code for the access token. When the application makes the request for the access token, that request can be authenticated with the client secret, which reduces the risk of an attacker intercepting the authorization code and using it themselves. This also means the access token is never visible to the user or their browser, so it is the most secure way to pass the token back to the application, reducing the risk of the token leaking to someone else.

The first step of the web flow is to request authorization from the user. This is accomplished by creating an authorization request link for the user to click on.

The authorization URL is usually in a format such as:

```
https://authorization-server.com/oauth/authorize
?client_id=a17c21ed
&response_type=code
&state=5ca75bd30
&redirect_uri=https%3A%2F%2Fexample-app.com%2Fauth
&scope=photos
```

The exact URL endpoint will be specified by the service to which you are connecting, but the parameter names will always be the same.

Note that you will most likely first need to register your redirect URL at the service before it will be accepted. This also means you can’t change your redirect URL per request. Instead, you can use the `state` parameter to customize the request. See below for more information.

After the user visits the authorization page, the service shows the user an explanation of the request, including application name, scope, etc. (See “approves the request” for an example screenshot.) If the user clicks “approve”, the server will redirect back to the app, with a “code” and the same “state” parameter you provided in the query string parameter. It is important to note that this is not an access token. The only thing you can do with the authorization code is to make a request to get an access token.

## OAuth Security

Up until 2019, the OAuth 2.0 spec only recommended using the PKCE extension for mobile and JavaScript apps. The latest OAuth Security BCP now recommends using PKCE also for server-side apps, as it provides some additional benefits there as well. It is likely to take some time before common OAuth services adapt to this new recommendation, but if you’re building a server from scratch you should definitely support PKCE for all types of clients.

## Authorization Request Parameters

The following parameters are used to make the authorization request. You should build a query string with the below parameters, appending that to the application’s authorization endpoint obtained from its documentation.

#### response_type=code

`response_type` is set to code indicating that you want an authorization code as the response.

#### client_id

The `client_id` is the identifier for your app. You will have received a client_id when first registering your app with the service.

#### redirect_uri (optional)

The `redirect_uri` may be optional depending on the API, but is highly recommended. This is the URL to which you want the user to be redirected after the authorization is complete. This must match the redirect URL that you have previously registered with the service.

#### scope (optional)

Include one or more scope values (space-separated) to request additional levels of access. The values will depend on the particular service.

#### state

The state parameter serves two functions. When the user is redirected back to your app, whatever value you include as the state will also be included in the redirect. This gives your app a chance to persist data between the user being directed to the authorization server and back again, such as using the state parameter as a session key. This may be used to indicate what action in the app to perform after authorization is complete, for example, indicating which of your app’s pages to redirect to after authorization.

The state parameter also serves as a CSRF protection mechanism if it contains a random value per request. When the user is redirected back to your app, double check that the state value matches what you set it to originally.

#### PKCE

If the service supports PKCE for web server apps, include the PKCE challenge and challenge method here as well. This is described in a complete example in Single-Page Apps and Mobile Apps.

Combine all of these query string parameters into the authorization URL, and direct the user’s browser there. Typically apps will put these parameters into a login button, or will send this URL as an HTTP redirect from the app’s own login URL.

## The user approves the request

After the user is taken to the service and sees the request, they will either allow or deny the request. If they allow the request, they will be redirected back to the redirect URL specified along with an authorization code in the query string. The app then needs to exchange this authorization code for an access token.

## Exchange the authorization code for an access token

To exchange the authorization code for an access token, the app makes a POST request to the service’s token endpoint. The request will have the following parameters.

#### grant_type (required)

The `grant_type` parameter must be set to “authorization_code”.

#### code (required)

This parameter is for the authorization code received from the authorization server which will be in the query string parameter “code” in this request.

#### redirect_uri (possibly required)

If the redirect URL was included in the initial authorization request, it must be included in the token request as well, and must be identical. Some services support registering multiple redirect URLs, and some require the redirect URL to be specified on each request. Check the service’s documentation for the specifics.

## Client Authentication (required)

The service will require the client authenticate itself when making the request for an access token. Typically services support client authentication via HTTP Basic Auth with the client’s `client_id` and `client_secret`. However, some services support authentication by accepting the `client_id` and `client_secret` as POST body parameters. Check the service’s documentation to find out what the service expects, since the OAuth 2.0 spec leaves this decision up to the service.

More advanced OAuth servers may also require other forms of client authentication such as mTLS or `private_key_jwt`. Refer to the service’s own documentation for those examples.

## PKCE Verifier

If the service supports PKCE for web server apps, then the client will need to include the followup PKCE parameter when exchanging the authorization code as well. Again, see Single-Page Apps and Mobile Apps for a complete example of using the PKCE extension.

[Previous](https://github.com/alithecodeguy/articles/blob/main/OAuth/OAuth%202.0%20Simplified/04%20Server-Side%20Apps/Server-SideApps_en.md "Previous")
/
[Next](https://github.com/alithecodeguy/articles/blob/main/OAuth/OAuth%202.0%20Simplified/04%20Server-Side%20Apps/02%20Example%20Flow/ExampleFlow_en.md "Next")
