<h1 align="center">Redirect URLs</h1>

Redirect URLs are a critical part of the OAuth flow. After a user successfully authorizes an application, the authorization server will redirect the user back to the application. Because the redirect URL will contain sensitive information, it is critical that the service doesn’t redirect the user to arbitrary locations.

The best way to ensure the user will only be redirected to appropriate locations is to require the developer to register one or more redirect URLs when they create the application. In these sections we will cover how to handle redirect URLs for mobile applications, how to validate redirect URLs, and how to handle errors.

- Redirect URI Registration
- Redirect URIs for Native Apps
- Redirect URI Validation

[Previous](https:// "Previous")
/
[Next](https:// "Next")
