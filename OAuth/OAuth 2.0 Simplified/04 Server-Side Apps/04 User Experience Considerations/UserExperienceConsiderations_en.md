<h1 align="center">User Experience Considerations</h1>

In order for the authorization code grant to be secure, the authorization page must appear in a web browser the user is familiar with, and must not be embedded in an iframe popup or an embedded browser in a mobile app. If it could be embedded in another website, the user would have no way of verifying it is the legitimate service and is not a phishing attempt.

If an app wants to use the authorization code grant but can’t protect its secret (i.e. native mobile apps or single-page JavaScript apps), then the client secret is not required when making a request to exchange the auth code for an access token, and PKCE must be used as well. However, some services still do not support PKCE, so it may not be possible to perform an authorization flow from the single-page app itself, and the client-side JavaScript code may need to have a companion server-side component that performs the OAuth flow instead.

[Previous](https://github.com/alithecodeguy/articles/blob/main/OAuth/OAuth%202.0%20Simplified/04%20Server-Side%20Apps/03%20Possible%20Errors/PossibleErrors_en.md "Previous")
/
[Next](https://github.com/alithecodeguy/articles/blob/main/OAuth/OAuth%202.0%20Simplified/04%20Server-Side%20Apps/05%20Security%20Considerations/SecurityConsiderations_en.md "Next")
