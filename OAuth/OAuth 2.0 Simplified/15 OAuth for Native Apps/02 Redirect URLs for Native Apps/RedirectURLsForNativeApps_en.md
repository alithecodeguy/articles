<h1 align="center">Redirect URLs for Native Apps</h1>

In order to support a wide range of types of native apps, your server will need to support registering three types of redirect URLs, each to support a slightly different use case.

## HTTPS URL Matching

Both iOS and Android allow apps to register URL patterns that indicate the app should be launched whenver a system browser visits a URL that matches the registered pattern. This is commonly used by apps to “deep link” into the native app, such as the Yelp app opening to the restaurant’s page when a Yelp URL is viewed in the browser.

This technique can also be used by apps to register URL a pattern that will launch the app when an authorization server redirects back to the app. If a platform provides this feature, this is the recommended choice for native apps, as this provides the most integrity that the app belongs to the URL it’s matching. This also provides a reasonable fallback in the case that the platform doesn’t support app-claimed URLs.

## Custom URL Scheme

Some platforms allow apps to register a custom URL scheme which will launch the app whenever a URL with that scheme is opened in a browser or another app. Supporting redirect URLs with a custom URL scheme allows clients to launch an external browser to complete the authorization flow, and then be redirected back to the application after the authorization is complete. However this method is less secure than the HTTPS URL matching method, as there is no global registry of custom URL schemes to avoid conflicts between developers.

App developers should choose a URL scheme that is likely to be globally unique, and one which they can assert control over. Since operating systems typically do not have a registry of whether a particular app has claimed a URL scheme, it is theoretically possible for two apps to independently choose the same scheme, such as `myapp://`. If you want to help prevent collisions by app developers using custom schemes, you should recommend (or even enforce) that they use a scheme that is the reverse domain name pattern of a domain they control. At the very least, you can require that the redirect URL contains at least one . so as not to conflict with other system schemes such as `mailto` or `ftp`.

For example, if an app has a corresponding website called `photoprintr.example.org`, the reverse domain name that can be used as their URL scheme would be `org.example.photoprintr`. The redirect URL that the developer would register would then begin with `org.example.photoprintr://`. By enforcing this, you can help encourage developers to choose explicit URL schemes that won’t conflict with other installed applications.

Apps that use a custom URL scheme will start the authorization request as normal, described in Authorization Request, but will provide a redirect URL that has their custom URL scheme. The authorization server should still verify that this URL was previously registered as an allowed redirect URL, and can treat it like any other redirect URL registered by web apps.

When the authorization server redirects the native app to the URL with the custom scheme, the operating system will launch the app and make the whole redirect URL accessible to the original app. The app can extract the authorization code just like a regular OAuth 2.0 client would.

## Loopback URLs

Another technique native applications may use for supporting seamless redirects is opening a new HTTP server on a random port of the loopback interface. This is typically only done on desktop operating systems or for command line applications, as mobile operating systems typically do not provide this functionality to app developers.

This approach works well for command line apps as well as desktop GUI apps. The app will start an HTTP server and then begin the authorization request, setting the redirect URL to a loopback address such as `http://127.0.0.1:49152/redirect` and launching a browser. When the authorization server redirects the browser back to the loopback address, the application can grab the authorization code from the request.

In order to suppor this use case, the authorization server will have to support registering redirect URLs beginning with `http://127.0.0.1:[port]/ `and `http://::1:[port]/`, and `http://localhost:[port]/`. The authorization server should allow an arbitrary path component as well as arbitrary port numbers. Note that in this case it is acceptable to use the HTTP scheme rather than HTTPS, as the request never leaves the device.

## Registration

As with server-side apps, native apps must also register their redirect URL(s) with the authorization server. This means the authorization server will need to allow registered redirect URLs that match all the patterns described above, in addition to traditional HTTPS URLs for server-side apps.

When the authorization request is initiated at the authorization server, the server will validate all the request parameters, including the redirect URL given. The authorization should reject unrecognized URLs in the request, to help avoid an authorization code interception attack.

[Previous](https:// "Previous")
/
[Next](https:// "Next")
