<h1 align="center">Use a System Browser</h1>

It used to be common practice for native apps to embed the OAuth interface in a web view inside the app. This approach has multiple problems, including that the client app can potentially eavesdrop on the user entering their credentials when signing in, or even present a false authorization page. Mobile operating system security is typically implemented in a way where the embedded web view doesn’t share cookies with the system’s native browser, so users have a worse experience because they need to enter their credentials each time as well.

The more secure and trusted way to accomplish the authorization flow is by launching a system browser. However, up until the addition of the specialized device APIs, this had the drawback of the user being popped out of the app and launching their browser, then redirecting back to the app, which is also not an ideal user experience.

Thankfully, the mobile platforms have been addressing the issue. There are now APIs available on iOS and Android for apps to launch a system browser but stay within the context of the application. The API does not not allow the client app to peek inside the browser, getting the security benefits of using an external browser and the user experience benefits of staying within the application the whole time.

Native app developers are strongly encouraged to use these special-purpose APIs, but if they can’t for some reason, fall back to launching an external browser instead of an embedded web view.

Authorization servers should enforce this behavior by attempting to detect whether the authorization URL was launched inside an embedded web view and reject the request if so. The particular techniques for detecting whether the page is being visited in an embedded web view vs the system browser will depend on the platform, but usually involve inspecting the user agent header.

[Previous](https:// "Previous")
/
[Next](https:// "Next")
