<h1 align="center">Password Grant</h1>

The Password grant is used when the application exchanges the user’s username and password for an access token. This is exactly the thing OAuth was created to prevent in the first place, so you should never allow third-party apps to use this grant.

Supporting the Password grant is very limiting, as there is no way to add multifactor authorization to this flow, and your options for detecting brute force attacks are more limited. This flow should not be used in practice.

The latest OAuth 2.0 Security Best Current Practice spec actually recommends against using the Password grant entirely, and it is being removed in the OAuth 2.1 update.

#### Request Parameters

The access token request will contain the following parameters.

- **grant_type** (required) – The grant_type parameter must be set to “password”.
- **username** (required) – The user’s username.
- **password** (required) – The user’s password.
- **scope** (optional) – The scope requested by the application.
- **Client Authentication** (required if the client was issued a secret)

If the client was issued a secret, then the client must authenticate this request. Typically the service will allow either additional request parameters `client_id` and `client_secret`, or accept the client ID and secret in the HTTP Basic Auth header.

## Example

The following is an example password grant the service would receive.

```
POST /oauth/token HTTP/1.1
Host: authorization-server.com

grant_type=password
&username=user@example.com
&password=1234luggage
&client_id=xxxxxxxxxx
&client_secret=xxxxxxxxxx
```

See _Access Token Response_ for details on the parameters to return when generating an access token or responding to errors.

[Previous](https:// "Previous")
/
[Next](https:// "Next")
