<h1 align="center">Redirect URLs for Native Apps</h1>

Native applications are clients installed on a device, such as a desktop application or native mobile application. There are a few things to keep in mind when supporting native apps related to security and user experience.

The authorization endpoint normally redirects the user back to the client’s registered redirect URL. Depending on the platform, native apps can either claim a URL pattern, or register a custom URL scheme that will launch the application. For example, an iOS application may register a custom protocol such as `myapp://` and then use a redirect_uri of `myapp://callback`.

## App-Claimed https URL Redirection

Some platforms, (Android, and iOS as of iOS 9), allow the app to override specific URL patterns to launch the native application instead of a web browser. For example, an application could register `https://app.example.com/auth` and whenever the web browser attempts to redirect to that URL, the operating system launches the native app instead.

If the operating system does support claiming URLs, this method should be used. If the operating system does some level of validation that the developer had control over this web URL, then this allows the identity of the native application to be guaranteed by the operating system. If the operating system does not support this, then the app will have to use a custom URL scheme instead.

## Custom URL Scheme

Most mobile and desktop operating systems allow apps to register a custom URL scheme that will launch the app when a URL with that scheme is visited from the system browser.

Using this method, the native app starts the OAuth flow as normal, by launching the system browser with the standard authorization code parameters. The only difference is that the redirect URL will be a URL with the app’s custom scheme.

When the authorization server sends the `Location` header intending to redirect the user to `myapp://callback#token=...`., the phone will launch the application and the app will be able to resume the authorization process, parsing the access token from the URL and storing it internally.

## Custom URL Scheme Namespaces

Since there is no centralized method of registering URL schemes, apps have to do their best to choose URL schemes that won’t conflict with each other.

Your service can help by requiring the URL scheme to follow a certain pattern, and only allow the developer to register a custom scheme that matches that pattern.

For example, Facebook generates a URL scheme for every app based on the app’s client ID. For example, `fb00000000://` where the numbers correspond to the app’s client ID. This provides a reasonably sure method of generating globally unique URL schemes, since other apps are unlikely to use a URL scheme with this pattern.

Another option for apps is to use the reverse domain name pattern with a domain that is under the control of the app’s publisher, resulting in a URL scheme of `com.example.myapp` for example. This is also something that can be enforced by the service if you wish.

[Previous](https:// "Previous")
/
[Next](https:// "Next")
