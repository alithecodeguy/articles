<h1 align="center">Access Token Response</h1>

## Successful Response

If the request for an access token is valid, the authorization server needs to generate an access token (and optional refresh token) and return these to the client, typically along with some additional properties about the authorization.

The response with an access token should contain the following properties:

- **access_token** (required) The access token string as issued by the authorization server.
- **token_type** (required) The type of token this is, typically just the string “Bearer”.
- **expires_in** (recommended) If the access token expires, the server should reply with the duration of time the access token is granted for.
- **refresh_token** (optional) If the access token will expire, then it is useful to return a refresh token which applications can use to obtain another access token. However, tokens issued with the implicit grant cannot be issued a refresh token.
- **scope** (optional) If the scope the user granted is identical to the scope the app requested, this parameter is optional. If the granted scope is different from the requested scope, such as if the user modified the scope, then this parameter is required.

When responding with an access token, the server must also include the additional `Cache-Control: no-store` HTTP header to ensure clients do not cache this request.

For example, a successful token response may look like the following:

```
HTTP/1.1 200 OK
Content-Type: application/json
Cache-Control: no-store

{
  "access_token":"MTQ0NjJkZmQ5OTM2NDE1ZTZjNGZmZjI3",
  "token_type":"Bearer",
  "expires_in":3600,
  "refresh_token":"IwOGYzYTlmM2YxOTQ5MGE3YmNmMDFkNTVk",
  "scope":"create"
}
```

## Access Tokens

The format for OAuth 2.0 Bearer tokens is actually described in a separate spec, RFC 6750. There is no defined structure for the token required by the spec, so you can generate a string and implement tokens however you want. The valid characters in a bearer token are alphanumeric, and the following punctuation characters:

```
-._~+/
```

A simple implementation of Bearer Tokens is to generate a random string and store it in a database along with the associated user and scope information, or more advanced systems may use self-encoded tokens where the token string itself contains all the necessary info.

## Unsuccessful Response

If the access token request is invalid, such as the redirect URL didn’t match the one used during authorization, then the server needs to return an error response.

Error responses are returned with an HTTP 400 status code (unless specified otherwise), with `error` and `error_description` parameters. The error parameter will always be one of the values listed below.

- **invalid_request** – The request is missing a parameter so the server can’t proceed with the request. This may also be returned if the request includes an unsupported parameter or repeats a parameter.
- **invalid_client** – Client authentication failed, such as if the request contains an invalid client ID or secret. Send an HTTP 401 response in this case.
- **invalid_grant** – The authorization code (or user’s password for the password grant type) is invalid or expired. This is also the error you would return if the redirect URL given in the authorization grant does not match the URL provided in this access token request.
- **invalid_scope** – For access token requests that include a scope (password or client_credentials grants), this error indicates an invalid scope value in the request.
- **unauthorized_client** – This client is not authorized to use the requested grant type. For example, if you restrict which applications can use the Implicit grant, you would return this error for the other apps.
- **unsupported_grant_type** – If a grant type is requested that the authorization server doesn’t recognize, use this code. Note that unknown grant types also use this specific error code rather than using the `invalid_request` above.

There are two optional parameters when returning an error response, `error_description` and `error_uri`. These are meant to give developers more information about the error, not intended to be shown to end users. However, keep in mind that many developers will pass this error text straight on to end users no matter how much you warn them, so it is a good idea to make sure it is at least somewhat helpful to end users as well.

The `error_description` parameter can only include ASCII characters, and should be a sentence or two at most describing the circumstance of the error. The `error_uri` is a great place to link to your API documentation for information about how to correct the specific error that was encountered.

The entire error response is returned as a JSON string, similar to the successful response. Below is an example of an error response.

```
HTTP/1.1 400 Bad Request
Content-Type: application/json
Cache-Control: no-store

{
  "error": "invalid_request",
  "error_description": "Request was missing the 'redirect_uri' parameter.",
  "error_uri": "See the full API docs at https://authorization-server.com/docs/access_token"
}
```

[Previous](https:// "Previous")
/
[Next](https:// "Next")
