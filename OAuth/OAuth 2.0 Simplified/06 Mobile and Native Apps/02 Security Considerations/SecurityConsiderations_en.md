<h1 align="center">Security Considerations</h1>

## Always use the secure embedded browser APIs, or launch a native browser

It is critical that the application use the appropriate browser APIs on the platforms rather than use embedded web views. On iOS, this is either `ASWebAuthenticationSession` or `SFSafariViewController`, and on Android this is known as “Custom Tabs”.

Using an embedded web view has many downsides, resulting in a higher likelihood of the user falling for a phishing attack since it provides no way for the user to verify the origin of the web page they’re looking at. It would be trivial for an attacker to create a web page that looks just like the authorization web page and embed it in their own malicious app, giving them the ability to steal usernames and passwords.

On the user experience side, using an embedded web view also has the downside of the web view not sharing system cookies so the user will be forced to enter their credentials every time. Instead, using the appropriate secure browser APIs will provide the opportunity for the user to bypass entering their credentials in the app if they’re already logged in to the authorization server in their browser.

[Previous](https: "Previous")
/
[Next](https: "Next")
