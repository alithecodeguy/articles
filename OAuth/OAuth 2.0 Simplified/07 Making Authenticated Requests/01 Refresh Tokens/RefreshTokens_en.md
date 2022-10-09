<h1 align="center">Refresh Tokens</h1>

When you initially received the access token, it may have included a refresh token as well as an expiration time like in the example below.

```
{
  "access_token": "AYjcyMzY3ZDhiNmJkNTY",
  "refresh_token": "RjY2NjM5NzA2OWJjuE7c",
  "token_type": "bearer",
  "expires": 3600
}
```

The presence of the refresh token means that the access token will expire and you’ll be able to get a new one without the user’s interaction.

The “expires_in” value is the number of seconds that the access token will be valid. It’s up to the service you’re using to decide how long access tokens will be valid, and may depend on the application or the organization’s own policies. You could use this timestamp to preemptively refresh your access tokens instead of waiting for a request with an expired token to fail. Some people like to get a new access token shortly before the current one will expire in order to save an HTTP request of an API call failing. While that is a perfectly fine optimization, it doesn’t stop you from still needing to handle the case where an API call fails if an access token expires before the expected time. Access tokens can expire for many reasons, such as the user revoking an app, or if the authorization server expires all tokens when a user changes their password.

If you make an API request and the token has expired already, you’ll get back a response indicating as such. You can check for this specific error message, and then refresh the token and try the request again.

If you’re using a JSON-based API, then it will likely return a JSON error response with the `invalid_token` error. In any case, the `WWW-Authenticate` header will also have the `invalid_token` error code.

```
HTTP/1.1 401 Unauthorized
WWW-Authenticate: Bearer error="invalid_token"
  error_description="The access token expired"
Content-type: application/json

{
  "error": "invalid_token",
  "error_description": "The access token expired"
}
```

When your application recognizes this specific error, it can then make a request to the token endpoint using the refresh token it previously received, and will get back a new access token it can use to retry the original request.

To use the refresh token, make a POST request to the service’s token endpoint with `grant_type=refresh_token`, and include the refresh token as well as the client credentials if required.

```
POST /oauth/token HTTP/1.1
Host: authorization-server.com

grant_type=refresh_token
&refresh_token=xxxxxxxxxxx
&client_id=xxxxxxxxxx
&client_secret=xxxxxxxxxx
```

The response will be a new access token, and optionally a new refresh token, just like you received when exchanging the authorization code for an access token.

```
{
  "access_token": "BWjcyMzY3ZDhiNmJkNTY",
  "refresh_token": "Srq2NjM5NzA2OWJjuE7c",
  "token_type": "Bearer",
  "expires": 3600
}
```

If you do not get back a new refresh token, then it means your existing refresh token will continue to work when the new access token expires.

The most secure option is for the authorization server to issue a new refresh token each time one is used. This is the recommendation in the latest Security Best Current Practice which enables authorization servers to detect if a refresh token is stolen. This is especially important for clients that don’t have a client secret, since the refresh token becomes the only thing needed to get new access tokens.

When the refresh token changes after each use, if the authorization server ever detects a refresh token was used twice, it means it has likely been copied and is being used by an attacker, and the authorization server can revoke all access tokens and refresh tokens associated with it immediately.

Keep in mind that at any point the user can revoke an application , so your application needs to be able to handle the case when using the refresh token also fails. At that point, you will need to prompt the user for authorization again, beginning a new OAuth flow from scratch.

You might notice that the “expires_in” property refers to the access token, not the refresh token. The expiration time of the refresh token is intentionally never communicated to the client. This is because the client has no actionable steps it can take even if it were able to know when the refresh token would expire. There are also many reasons refresh tokens may expire prior to any expected lifetime of them as well.

If a refresh token expires for any reason, then the only action the application can take is to ask the user to log in again, starting a new OAuth flow from scratch, which will issue a new access token and refresh token to the application. That’s the reason it doesn’t matter whether the application knows the expected lifetime of the refresh token, because regardless of the reason it expires the outcome is always the same.

[Previous](https:// "Previous")
/
[Next](https:// "Next")
