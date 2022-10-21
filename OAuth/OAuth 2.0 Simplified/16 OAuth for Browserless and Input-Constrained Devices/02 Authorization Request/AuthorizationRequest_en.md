<h1 align="center">Authorization Request</h1>

First, the device makes a request to the authorization server to request the device code, identifying itself with its client ID, and requesting one or more scopes if it needs to.

```
POST /token HTTP/1.1
Host: authorization-server.com
Content-type: application/x-www-form-urlencoded

client_id=a17c21ed
```

The authorization server responds with a JSON payload containing the device code, the code the user will enter, the URL the user should visit, and a polling interval.

```
HTTP/1.1 200 OK
Content-Type: application/json
Cache-Control: no-store
{
    "device_code": "NGU5OWFiNjQ5YmQwNGY3YTdmZTEyNzQ3YzQ1YSA",
    "user_code": "BDWP-HQPK",
    "verification_uri": "https://authorization-server.com/device",
    "interval": 5,
    "expires_in": 1800
}
```

The device shows the `verification_uri` and `user_code` to the user
on its display, directing the user to enter the code at that URL.

[Previous](https:// "Previous")
/
[Next](https:// "Next")
