<h1 align="center">OAuth for Native Apps</h1>

This chapter describes some special considerations to keep in mind when supporting OAuth for native apps. Like browser-based apps, native apps can’t use a client secret, as that would require that the developer ship the secret in their binary distribution of the application. It has been proven to be relatively easy to decompile and extract the secret. As such, native apps must use an OAuth flow that does not require a preregistered client secret.

The current industry best practice is to use the Authorization Flow along with the PKCE extension, omitting the client secret from the request, and to use an external user agent to complete the flow. An external user agent is typically the device’s native browser, (with a separate security domain from the native app) so that the app cannot access the cookie storage or inspect or modify the page content inside the browser. Since the app can’t reach inside the browser being used in this case, this provides the opportunity for the device to keep users signed in while authorizing different applications, so that they don’t have to enter their credentials each time they authorize a new application.

In recent years, both iOS and Android have been working to further improve the user experience of OAuth for native apps by providing a native user agent that can be launched from within the application, while still being isolated from the application launching it. The result is that the user no longer needs to leave the application in order to launch a native browser that shares the system cookies. This was first added as `SFSafariViewController` in iOS 9, and later evolved to `SFAuthenticationSession` in iOS 11, and `ASWebAuthenticationSession` in iOS 12.

These recommendations for native apps are published as an RFC 8252, where these concepts are described in more explicit detail.

- Use a System Browser
- Redirect URLs for Native Apps
- PKCE Extension
- Checklist for Server Support for Native Apps

[Previous](https:// "Previous")
/
[Next](https:// "Next")
