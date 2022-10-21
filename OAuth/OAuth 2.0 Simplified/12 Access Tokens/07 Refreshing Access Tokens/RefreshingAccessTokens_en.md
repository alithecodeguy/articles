<h1 align="center">Refreshing Access Tokens</h1>

This section describes how to allow your developers to use refresh tokens to obtain new access tokens. If your service issues refresh tokens along with the access token, then you’ll need to implement the Refresh grant type described here.

## Request Parameters

The access token request will contain the following parameters.

**grant_type** (required)
The grant_type parameter must be set to “refresh_token”.

**refresh_token** (required)
The refresh token previously issued to the client.

**scope** (optional)
The requested scope must not include additional scopes that were not issued in the original access token. Typically this will not be included in the request, and if omitted, the service should issue an access token with the same scope as was previously issued.

**Client Authentication** (required if the client was issued a secret)
Typically, refresh tokens are only used with confidential clients. However, since it is possible to use the authorization code flow without a client secret, the refresh grant may also be used by clients that don’t have a secret. If the client was issued a secret, then the client must authenticate this request. Typically the service will allow either additional request parameters `client_id` and `client_secret`, or accept the client ID and secret in the HTTP Basic auth header. If the client does not have a secret, then no client authentication will be present in this request.

## Verifying the refresh token grant

After checking for all required parameters, and authenticating the client if the client was issued a secret, the authorization server can continue verifying the other parts of the request.

The server then checks whether the refresh token is valid, and has not expired. If the refresh token was issued to a confidential client, the service must ensure the refresh token in the request was issued to the authenticated client.

If everything checks out, the service can generate an access token and respond. The server may issue a new refresh token in the response, but if the response does not include a new refresh token, the client assumes the existing refresh token will still be valid.

## ExampleExample

The following is an example refresh grant the service would receive.

```
POST /oauth/token HTTP/1.1
Host: authorization-server.com

grant_type=refresh_token
&refresh_token=xxxxxxxxxxx
&client_id=xxxxxxxxxx
&client_secret=xxxxxxxxxx
```

## Response

The response to the refresh token grant is the same as when issuing an access token. You can optionally issue a new refresh token in the response, or if you don’t include a new refresh token, the client assumes the current refresh token will continue to be valid.

[Previous](https:// "Previous")
/
[Next](https:// "Next")
