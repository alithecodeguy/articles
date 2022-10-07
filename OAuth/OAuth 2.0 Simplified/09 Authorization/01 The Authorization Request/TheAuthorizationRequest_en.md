<h1 align="center">Authorization </h1>

Clients will direct a user’s browser to the authorization server to begin the OAuth process. Clients may use either the authorization code grant type or the implicit grant. Along with the type of grant specified by the `response_type` parameter, the request will have a number of other parameters to indicate the specifics of the request.

Server-Side Apps describes how clients will build the authorization URL for your service. The first time the authorization server sees the user will be this authorization request, the user will be directed to the server with the query parameters the client has set. At this point, the authorization server will need to validate the request and present the authorization interface, allowing the user to approve or deny the request.

## Request Parameters

The following parameters are used to begin the authorization request. For example, if the authorization server URL is https://authorization-server.com/auth then the client will craft a URL like the following and direct the user’s browser to it:

```
https://authorization-server.com/auth?response_type=code
&client_id=29352735982374239857
&redirect_uri=https://example-app.com/callback
&scope=create+delete
&state=xcoivjuywkdkhvusuye3kch
```

#### response_type

`response_type` will be set to code, indicating that the application expects to receive an authorization code if successful.

#### client_id

The `client_id` is the public identifier for the app.

#### redirect_uri (optional)

The `redirect_uri` is not required by the spec, but your service should require it. This URL must match one of the URLs the developer registered when creating the application, and the authorization server should reject the request if it does not match.

#### scope (optional)

The request may have one or more scope values indicating additional access requested by the application. The authorization server will need to display the requested scopes to the user.

#### state (recommended)

The `state` parameter is used by the application to store request-specific data and/or prevent CSRF attacks. The authorization server must return the unmodified state value back to the application.

#### PKCE

If the authorization server supports the PKCE extension (described in PKCE) then the `code_challenge` and `code_challenge_method` parameters will also be present. These must be remembered by the authorization server between issuing the authorization code and issuing the access token.

## Verifying the Authorization Request

The authorization server must first verify that the `client_id` in the request corresponds to a valid application.

If your server allows applications to register more than one redirect URL, then there are two steps to validating the redirect URL. If the request contains a `redirect_uri` parameter, the server must confirm it is a valid redirect URL for this application. If there is no `redirect_uri` parameter in the request, and only one URL was registered, the server uses the redirect URL that was previously registered. Otherwise, if no redirect URL is in the request, and no redirect URL has been registered, this is an error.

If the `client_id` is invalid, the server should reject the request immediately and display the error to the user rather than redirecting the user back to the application.

## Invalid Redirect URL

If the authorization server detects a problem with the redirect URL, it needs to inform the user of the problem instead of redirecting the user. The redirect URL could be invalid for a number of reasons, including:

- the redirect URL parameter is missing
- the redirect URL parameter was invalid, such as if it was a string that does not parse as a URL
- the redirect URL does not match one of the registered redirect URLs for the application

In these cases, the authorization server should display an error to the user informing them of the problem. The server must not redirect the user back to the application. This avoids what is known as an “open redirector attack.” The server should only redirect the user to the redirect URL if the redirect URL has been registered.

## Other Errors

All other errors should be handled by redirecting the user to the redirect URL with an error code in the query string. See the Authorization Response section for details on how to respond with an error.

If the request is missing the `response_type` parameter, or the value of that parameter is anything besides code or token, the server can return an `invalid_request` error.

Since the authorization server may require clients to specify if they are public or confidential, it can reject authorization requests that aren’t allowed. For example, if the client specified they are a confidential client, the server can reject a request that uses the token grant type. When rejecting for this reason, use the error code `unauthorized_client`.

The authorization server should reject the request if there are scope values that it doesn’t recognize. In this case, the server can redirect to the callback URL with the `invalid_scope` error code.

The authorization server needs to store the “state” value (and PKCE values) for this request in order to include it in the authorization response. The server must not modify or make any assumptions about what the state value contains, since it is purely for the benefit of the client.

[Previous](https: "Previous")
/
[Next](https: "Next")
