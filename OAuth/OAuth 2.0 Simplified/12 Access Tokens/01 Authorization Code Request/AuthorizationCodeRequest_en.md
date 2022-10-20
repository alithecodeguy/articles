<h1 align="center">Authorization Code Request</h1>

The authorization code grant is used when an application exchanges an authorization code for an access token. After the user returns to the application via the redirect URL, the application will get the authorization code from the URL and use it to request an access token. This request will be made to the token endpoint.

## Request Parameters

The access token request will contain the following parameters.

#### grant_type (required)

The `grant_type` parameter must be set to “authorization_code”.

#### code (required)

This parameter is the authorization code that the client previously received from the authorization server.

#### redirect_uri (possibly required)

If the redirect URI was included in the initial authorization request, the service must require it in the token request as well. The redirect URI in the token request must be an exact match of the redirect URI that was used when generating the authorization code. The service must reject the request otherwise.

#### code_verifier (required for PKCE support)

If the client included a `code_challenge` parameter in the initial authorization request, it must now prove it has the secret used to generate the hash by sending it in the POST request. This is the plaintext string that was used to calculate the hash that was previously sent in the `code_challenge` parameter.

#### client_id (required if no other client authentication is present)

If the client is authenticating via HTTP Basic Auth or some other method, then this parameter is not required. Otherwise, this parameter is required.

If the client was issued a client secret, then the server must authenticate the client. One way to authenticate the client is to accept another parameter in this request, `client_secret`. Alternately the authorization server can use HTTP Basic Auth. Technically the spec allows the authorization server to support any form of client authentication, and mentions public/private key pair as an option. In practice, most consumer servers support the simpler methods of authenticating clients using either or both of the methods mentioned here. For more advanced methods of authenticating the client, refer to RFC 7523 which defines a method of using a signed JWT as client authentication.

#### Verifying the authorization code grant

After checking for all required parameters, and authenticating the client if the client was issued credentials, the authorization server can continue verifying the other parts of the request.

The server then checks if the authorization code is valid, and has not expired. The service must then verify that the authorization code provided in the request was issued to the client identified. Lastly, the service must ensure the redirect URI parameter present matches the redirect URI that was used to request the authorization code.

For PKCE support, the authorization server should calculate the SHA256 hash of the `code_verifier` presented in this token request, and compare that with the `code_challenge` presented in the authorization request. If they match, the authorization server can be confident that it’s the same client making this token request that made the original authorization request.

If everything checks out, the service can generate an access token and respond.

## Example

The following example shows an authorization grant request for a confidential client.

```
POST /oauth/token HTTP/1.1
Host: authorization-server.com

grant_type=authorization_code
&code=xxxxxxxxxxx
&redirect_uri=https://example-app.com/redirect
&code_verifier=Th7UHJdLswIYQxwSg29DbK1a_d9o41uNMTRmuH0PM8zyoMAQ
&client_id=xxxxxxxxxx
&client_secret=xxxxxxxxxx
```

See Access Token Response for details on the parameters to return when generating an access token or responding to errors.

## Security Considerations

#### Preventing replay attacks

If an authorization code is used more than once, the authorization server must deny the subsequent requests. This is easy to accomplish if the authorization codes are stored in a database, since they can simply be marked as used.

If you are implementing self-encoded authorization codes, as in our example code, you’ll need to keep track of the tokens that have been used for the lifetime of the token. One way to accomplish this by caching the code in a cache for the lifetime of the code. This way when verifying codes, we can first check if they have already been used by checking the cache for the code. Once the code reaches its expiration date, it will no longer be in the cache, but we can reject it based on the expiration date anyway.

If a code is used more than once, it should be treated as an attack. If possible, the service should revoke the previous access tokens that were issued from this authorization code.

[Previous](https:// "Previous")
/
[Next](https:// "Next")
