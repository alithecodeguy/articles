<h1 align="center">Making Authenticated Requests</h1>

Regardless of which grant type you used or whether you used a client secret, you now have an OAuth 2.0 Bearer Token you can use with the API.

The access token is sent to the service in the HTTP `Authorization` header prefixed by the text `Bearer`. Historically, some services allowed the token to be sent in the post body parameter or even the GET query string, but there are downsides to these approaches and for the most part modern implementations will use only the HTTP header method.

When passing in the access token in an HTTP header, you should make a request like the following:

```
POST /resource/1/update HTTP/1.1
Authorization: Bearer RsT5OjbzRn430zqMLgV3Ia"
Host: api.authorization-server.com

description=Hello+World
```

The access token is not intended to be parsed or understood by your application. The only thing your application should do with it is use it to make API requests. Some services will use structured tokens like JWTs as their access tokens, described in Self-Encoded Access Tokens but the client does not need to worry about decoding the token in this case.

In fact, attempting to decode the access token is dangerous, as the server makes no guarantees that access tokens will always continue to be in the same format. It’s entirely possible that the next time you get an access token from the service, it will be in a different format. The thing to keep in mind is that access tokens are opaque to the client, and should only be used to make API requests and not interpreted themselves.

If you are trying to find out whether your access token has expired, you can either store the expiration lifetime that was returned when you first got the access token, or just try to make the request anyway, and get a new access token if the current one has expired. In practice, there isn’t much of a difference. While preemptively refreshing the access token can save an HTTP request, you still need to handle the case when an API call reports an expired token before you were expecting it to expire, since access tokens can expire for many reasons beyond just their expected lifetime.

See below for more details on getting new access tokens using refresh tokens.

If you’re trying to find out more information about the user who signed in, you should read the API docs of the particular service to find out their recommendation. For example, Google’s API uses OpenID Connect to provide a userinfo endpoint that can return info about the user given an access token, or you can get the user’s information from an ID token instead. We walk through a complete example of the userinfo endpoint workflow in Signing in with Google.

[Previous](https: "Previous")
/
[Next](https: "Next")
