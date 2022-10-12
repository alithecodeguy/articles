<h1 align="center">The Authorization Response</h1>

Once the user has finished logging in and approving the request, the authorization server is ready to redirect the user back to the application.

## Authorization Code Response

If the request is valid and the user grants the authorization request, the authorization server generates an authorization code and redirects the user back to the application, adding the authorization code and the application’s “state” value to the redirect URL.

## Generating the Authorization Code

The authorization code must expire shortly after it is issued. The OAuth 2.0 spec recommends a maximum lifetime of 10 minutes, but in practice, most services set the expiration much shorter, around 30-60 seconds. The authorization code itself can be of any length, but the length of the codes should be documented.

Because authorization codes are meant to be short-lived and single-use, you could implement them as self encoded tokens. With this technique, you can avoid storing authorization codes in a database, and instead, encode all of the necessary information into the authorization code itself. You can use either a built-in encryption library of your server-side environment, or a standard such as JSON Web Signature (JWS). However, since this authorization code is only meant to be used by the authorization server, it can often be simpler to implement them as short strings stored in a server-side cache that’s accessible to the authorization endpoint and token endpoint.

In any case, the information that will need to be associated with the authorization code is the following.

- client_id – The client ID (or other client identifier) that requested this code
- redirect_uri – The redirect URL that was used. This needs to be stored since the access token request must contain the same redirect URL for verification when issuing the access token.
- User info – Some way to identify the user that this authorization code is for, such as a user ID.
- Expiration Date – The code needs to include an expiration date so that it only lasts a short time.
- Unique ID – The code needs its own unique ID of some sort in order to be able to check if the code has been used before. A database ID or a random string is sufficient.
- PKCE: code_challenge and code_challenge_method – When supporting PKCE, these two values provided by the application need to be stored so that they can be verified when issuing the access token later.

Once you’ve generated the authorization code, either by creating a JWS-encoded string, or by generating a random string and storing the associated information in a database, you’ll need to redirect the user to the application’s redirect URL specified. The parameters to be added to the query string of the redirect URL are as follows:

#### code

This parameter contains the authorization code which the client will later exchange for an access token.

#### state

If the initial request contained a state parameter, the response must also include the exact value from the request. The client will be using this to associate this response with the initial request.

For example, the authorization server redirects the user by sending the following HTTP response.

```
HTTP/1.1 302 Found
Location: https://example-app.com/redirect?code=g0ZGZmNjVmOWI&state=dkZmYxMzE2
```

## Implicit Grant Type Response

With the Implicit grant (`response_type=token`) the authorization server generates an access token immediately and redirects to the callback URL with the token and other access token attributes in the fragment.

For example, the authorization server redirects the user by sending the following HTTP response (extra line breaks for display purposes).

```
HTTP/1.1 302 Found
Location: https://example-app.com/redirect#access_token=MyMzFjNTk2NTk4ZTYyZGI3
 &state=dkZmYxMzE2
 &token_type=Bearer
 &expires_in=86400
```

You can see that this is much more dangerous than issuing a temporary one-time-use authorization code. Since there are many more ways an attacker can steal data out of an HTTP redirect compared to intercepting an HTTPS request, it’s much riskier using this option compared to the authorization code flow.

From the authorization server’s point of view, at the point it creates the access token and sends the HTTP redirect, it has no way of knowing whether or not the redirect was successful and the correct application has received the access token. It’s kind of tossing the access token up into the air and crossing its fingers that the app catches it. This is in contrast to the authorization code method, where even though the authorization server can’t guarantee the authorization code wasn’t stolen, it can at least prevent a stolen authorization code from being useful by requiring a client secret or the PKCE code verifier. This provides a much greater level of security since the authorization server can now be more confident that it won’t be giving access tokens away to attackers.

For these reasons as well as more documented in OAuth 2.0 for Browser-Based Apps, it is recommended that the Implicit flow no longer be used.

## Error Response

There are two different kinds of errors to handle. The first kind of error is when the developer did something wrong when creating the authorization request. The other kind of error is when the user rejects the request (clicks the “Deny” button).

If there is something wrong with the syntax of the request, such as the `redirect_uri` or `client_id` is invalid, then it’s important not to redirect the user and instead you should show the error message directly. This is to avoid letting your authorization server be used as an open redirector.

If the `redirect_uri` and `client_id` are both valid, but there is still some other problem, it’s okay to redirect the user back to the redirect URI with the error in the query string.

When redirecting back to the application to indicate an error, the server adds the following parameters to the redirect URL:

#### error

a single ASCII error code from the following list:

- **invalid_request** – the request is missing a parameter, contains an invalid parameter, includes a parameter more than once, or is otherwise invalid.
- **access_denied** – the user or authorization server denied the request
- **unauthorized_client** – the client is not allowed to request an authorization code using this method, for example if a confidential client attempts to use the implicit grant type.
- **unsupported_response_type** – the server does not support obtaining an authorization code using this method, for example if the authorization server never implemented the implicit grant type.
- **invalid_scope** – the requested scope is invalid or unknown.
- **server_error** – instead of displaying a 500 Internal Server Error page to the user, the server can redirect with this error code.
- **temporarily_unavailable** – if the server is undergoing maintenance, or is otherwise unavailable, this error code can be returned instead of responding with a 503 Service Unavailable status code.

#### error_description

The authorization server can optionally include a human-readable description of the error. This parameter is intended for the developer to understand the error, and is not meant to be displayed to the end user. The valid characters for this parameter are the ASCII character set except for the double quote and backslash, specifically, hex codes 20-21, 23-5B and 5D-7E.

#### error_uri

The server can also return a URL to a human-readable web page with information about the error. This is intended for the developer to get more information about the error, and is not meant to be displayed to the end user.

#### state

If the request contained a state parameter, the error response must also include the exact value from the request. The client may use this to associate this response with the initial request.

#### Example

For example, if the user denied the authorization request, the server would construct the following URL and send an HTTP redirect response like the below (newlines in the URL are for illustration purposes).

```
HTTP/1.1 302 Found
Location: https://example-app.com/redirect?error=access_denied
 &error_description=The+user+denied+the+request
 &error_uri=https%3A%2F%2Foauth2server.com%2Ferror%2Faccess_denied
 &state=wxyz1234
```

[Previous](https:// "Previous")
/
[Next](https:// "Next")
