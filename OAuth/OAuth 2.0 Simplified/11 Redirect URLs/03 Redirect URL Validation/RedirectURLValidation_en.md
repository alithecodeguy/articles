<h1 align="center">Redirect URL Validation</h1>

There are three cases when you’ll need to validate redirect URLs.

- When the developer registers the redirect URL as part of creating an application
- In the authorization request (both authorization code and implicit grant types)
- When the application exchanges an authorization code for an access token

## Redirect URL Registration

As discussed in _Creating an Application_, the service should allow developers to register one or more redirect URLs when creating the application. The only restriction on the redirect URL is that it cannot contain a fragment component. The service must allow developers to register redirect URLs with custom URL schemes, in order to support _native applications_ on some platforms.

## Authorization Request

When the application starts the OAuth flow, it will direct the user to your service’s authorization endpoint. The request will have several parameters in the URL, including a redirect URL.

At this point, the authorization server must validate the redirect URL to ensure the URL in the request matches one of the registered URLs for the application. The request will also have a client_id parameter, so the service should look up the redirect URLs based on that. It is entirely possible for an attacker to craft an authorization request with one app’s `client ID` and the attacker’s redirect URL, which is why registration is required.

The service should look for an exact match of the URL, and avoid matching on only part of the specific URL. (The client can use the state parameter if it needs to customize each request.) Simple string matching is sufficient since the redirect URL can’t be customized per request. All the server needs to do is check that the redirect URL in the request matches one of the redirect URLs the developer entered when registering their application.

If the redirect URL is not one of the registered redirect URLs, then the server must immediately show an error indicating such, and not redirect the user. This avoids having your authorization server be used as an open redirector.

## Granting Access Tokens

The token endpoint will get a request to exchange an authorization code for an access token. This request will contain a redirect URL as well as the authorization code. As an added measure of security, the server should verify that the redirect URL in this request matches exactly the redirect URL that was included in the initial authorization request for this authorization code. If the redirect URL does not match, the server rejects the request with an error.

[Previous](https:// "Previous")
/
[Next](https:// "Next")
