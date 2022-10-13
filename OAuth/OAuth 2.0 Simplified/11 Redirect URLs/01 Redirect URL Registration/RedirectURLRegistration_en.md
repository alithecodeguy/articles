<h1 align="center">Redirect URL Registration</h1>

In order to avoid exposing users to open redirector attacks, you must require developers register one or more redirect URLs for the application. The authorization server must never redirect to any other location. Registering a New Application covers creating a registration form to allow developers to register redirect URLs for their applications.

If an attacker can manipulate the redirect URL before the user reaches the authorization server, they could cause the server to redirect the user to a malicious server which would send the authorization code to the attacker. This is one way attackers can try to intercept an OAuth exchange and steal access tokens. If the authorization endpoint does not limit the URLs that it will redirect to, then it’s considered an “open redirector”, and can be used in combination with other things to launch attacks that aren’t even related to OAuth necessarily.

## Valid Redirect URLs

When you build the form to allow developers to register redirect URLs, you should do some basic validation of the URL that they enter.

Registered redirect URLs may contain query string parameters, but must not contain anything in the fragment. The registration server should reject the request if the developer tries to register a redirect URL that contains a fragment.

Note that for native and mobile apps, the platform may allow a developer to register a URL scheme such as myapp:// which can then be used in the redirect URL. This means the authorization server should allow arbitrary URL schemes to be registered in order to support registering redirect URLs for native apps. See Mobile and Native Apps for more information.

## Per-Request Customization

Often times a developer will think that they need to be able to use a different redirect URL on each authorization request, and will try to change the query string parameters per request. This is not the intended use of the redirect URL, and should not be allowed by the authorization server. The server should reject any authorization requests with redirect URLs that are not an exact match of a registered URL.

If a client wishes to include request-specific data in the redirect URL, it can instead use the “state” parameter to store data that will be included after the user is redirected. It can either encode the data in the state parameter itself, or use the state parameter as a session ID to store the state on the server.

[Previous](https:// "Previous")
/
[Next](https:// "Next")
