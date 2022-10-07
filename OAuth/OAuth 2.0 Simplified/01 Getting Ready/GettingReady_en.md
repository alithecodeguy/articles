<h1 align="center">Getting Ready</h1>

In Part I of this book, we’ll walk through the things you need to know when you’re building an app that talks to an existing OAuth 2.0 API. Whether you’re building a web app or a mobile app, there are a few things you’ll need to keep in mind as we get started.

Every OAuth 2.0 service will require that you first register a new application, which also typically requires that you first sign up as a developer with the service.

## Creating an Application

The registration process typically involves creating a developer account on the service’s website, then entering basic information about the application such as the name, website, logo, etc. After registering the application, you’ll be given a `client_id` (and a `client_secret` in some cases) that you’ll use when your app interacts with the service.

One of the most important things when creating the application is to register one or more redirect URLs the application will use. The redirect URLs are where the OAuth 2.0 service will return the user to after they have authorized the application. It is critical that these are registered, otherwise it is easy to create malicious applications that can steal user data. This is covered in more detail later in this book.

## Redirect URLs and State

OAuth 2.0 APIs will only redirect users to a URL that was previously registered with the service, in order to prevent redirection attacks where an authorization code or access token can be intercepted by an attacker. Some services may allow you to register multiple redirect URLs, which can help when your web app may be running on serveral different subdomains.

In order to be secure, the redirect URL must be an https endpoint to prevent the authorization code from being intercepted during the authorization process. If your redirect URL is not https, then an attacker may be able to intercept the authorization code and use it to hijack a session. The one exception to this is for apps running on the loopback interface, such as a native desktop application, or when doing local development. However even though the spec allows this exception, some OAuth services you encounter may require https redirect URLs anyway.

OAuth services should be looking for an exact match of the redirect URL. This means a redirect URL of https://example.com/auth would not match https://example.com/auth?destination=account. It is best practice to avoid using query string parameters in your redirect URL, and have it include just a path.

Some applications may have multiple places they want to start the OAuth process from, such as a login link on a home page as well as a login link when viewing some public item. For these applications, it may be tempting to try to register multiple redirect URLs, or you may think you need to be able to vary the redirect URL per request. Instead, OAuth 2.0 provides a mechanism for this, the “state” parameter.

The “state” parameter can be used to encode application state, but it must also include some amount of random data if you’re not also including PKCE parameters in the request. The state parameter is a string that is opaque to the OAuth 2.0 service, so whatever state value you pass in during the initial authorization request will be returned after the user authorizes the application. You could for example encode a redirect URL in something like a JWT, and parse this after the user is redirected back to your application so you can take the user back to the appropriate location after they sign in.

Note that unless you are using a signed or encrypted method like JWT to encode the state parameter, you should treat it as untrusted/unvalidated data when it arrives at your redirect URL, since it’s trivial for anyone to modify that parameter on the redirect back to your app.

[Previous](https://github.com/alithecodeguy/articles/blob/main/OAuth/OAuth%202.0%20Simplified/00%20Background/Background_en.md "Previous")
[Next](https://github.com/alithecodeguy/articles/blob/main/OAuth/OAuth%202.0%20Simplified/02%20Accessing%20Data%20in%20an%20OAuth%20Server/AccessingDataInAnOAuthServer_en.md "Next")
