<h1 align="center">Token Request</h1>

While the device is waiting for the user to complete the authorization flow on their own computer or phone, the device meanwhile begins polling the token endpoint to request an access token.

The device makes a POST request with the `device_code` at the rate specified by `interval`. The device should continue requesting an access token until a response other than `authorization_pending` is returned, either the user grants or denies the request or the device code expires.

```
POST /token HTTP/1.1
Host: authorization-server.com
Content-type: application/x-www-form-urlencoded

grant_type=urn:ietf:params:oauth:grant-type:device_code&
client_id=a17c21ed&
device_code=NGU5OWFiNjQ5YmQwNGY3YTdmZTEyNzQ3YzQ1YSA
```

The authorization server will reply with either an error or an access token. The Device Flow spec defines two additional error codes beyond what is defined in OAuth 2.0 core, `authorization_pending` and `slow_down`.

If the device is polling too frequently, the authorization server will return the `slow_down` error.

```
HTTP/1.1 400 Bad Request
Content-Type: application/json
Cache-Control: no-store

{
  "error": "slow_down"
}
```

If the user has not either allowed or denied the request yet, the authorization server will return the `authorization_pending` error.

```
HTTP/1.1 400 Bad Request
Content-Type: application/json
Cache-Control: no-store

{
  "error": "authorization_pending"
}
```

If the user denies the request, the authorization server will return the `access_denied` error.

```
HTTP/1.1 400 Bad Request
Content-Type: application/json
Cache-Control: no-store

{
  "error": "access_denied"
}
```

If the device code has expired, the authorization server will return the `expired_token` error. The device can immediately make a request for a new device code.

```
HTTP/1.1 400 Bad Request
Content-Type: application/json
Cache-Control: no-store

{
  "error": "expired_token"
}
```

Finally, if the user allows the request, then the authorization server issues an access token like normal and returns the standard access token response.

```
HTTP/1.1 200 OK
Content-Type: application/json
Cache-Control: no-store

{
  "access_token": "AYjcyMzY3ZDhiNmJkNTY",
  "refresh_token": "RjY2NjM5NzA2OWJjuE7c",
  "token_type": "Bearer",
  "expires": 3600,
  "scope": "create"
}
```

[Previous](https:// "Previous")
/
[Next](https:// "Next")
