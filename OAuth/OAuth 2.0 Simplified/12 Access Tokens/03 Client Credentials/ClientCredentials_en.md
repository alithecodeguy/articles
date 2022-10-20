<h1 align="center">Client Credentials</h1>

The Client Credentials grant is used when applications request an access token to access their own resources, not on behalf of a user.

## Request Parameters

**grant_type** (required)
The `grant_type` parameter must be set to `client_credentials`.

**scope** (optional)
Your service can support different scopes for the client credentials grant. In practice, not many services actually support this.

**Client Authentication** (required)
The client needs to authenticate themselves for this request. Typically the service will allow either additional request parameters `client_id` and `client_secret`, or accept the client ID and secret in the HTTP Basic auth header.

## Example

The following is an example authorization code grant the service would receive.

```
POST /token HTTP/1.1
Host: authorization-server.com

grant_type=client_credentials
&client_id=xxxxxxxxxx
&client_secret=xxxxxxxxxx
```

See Access Token Response for details on the parameters to return when generating an access token or responding to errors.

[Previous](https:// "Previous")
/
[Next](https:// "Next")
