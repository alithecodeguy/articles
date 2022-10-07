<h1 align="center">Signing in with Google</h1>

Despite OAuth being an _authorization_ protocol rather than an _authentication_ protocol, it is often used as the basis for authentication workflows anyway. A typical use of many common OAuth APIs is just to identify the user at the computer when logging in to a third-party app.

Authentication and authorization are often confused with each other, but can be more easily understood if you think about them from the perspective of an application. An app that is authenticating users is just verifying who the user is. An app that is authorizing users is trying to gain access or modify something that belongs to the user.

OAuth was designed as an authorization protocol, so the end result of every OAuth flow is the app obtains an access token in order to be able to access or modify something about the user’s account. The access token itself says nothing about who the user is.

There are several ways different services provide a way for an app to find out the identity of the user. A simple way is for the API to provide a “user info” endpoint which will return the authenticated user’s name and other profile info when an API call is made with an access token. While this is not something that is part of the OAuth standard, it’s a common approach many services have taken. A more advanced and standardized approach is to use OpenID Connect, an OAuth 2.0 extension. OpenID Connect is covered in more detail in .

This chapter will walk through using a simplified OpenID Connect workflow with the Google API to identify the user who signed in to your application.

[Previous](https: "Previous")
/
[Next](https: "Next")
