<h1 align="center">Security Considerations for Single-Page Apps</h1>

With browser-based apps there is always a risk of things like Cross-Site Scripting (XSS) attacks due to the increased attack surface and number of moving parts in websites. Additionally, browsers currently don’t have a secure storage mechanism available to store things like the access token or refresh token. As such, browsers are always considered a higher risk in an OAuth deployment compared to other platforms, and the authorization server will usually have special policies around token lifetimes to mitigate that risk.

## Refresh Tokens

Historically, with the Implicit flow, there was never any mechanism for returning refresh tokens to JavaScript applications. This made sense at the time, because it was well known that the Implicit flow was less secure, and without a client secret, a refresh token can be used to get new access tokens indefinitely, so this would be an even greater risk than a leaked access token. There was also little need for a refresh token since JavaScript apps would only be running when the user was actively using the browser, so they could redirect to the authorization server to get a new access token if needed.

With the developments over the last few years of adopting PKCE for JavaScript apps, there is now the potential for refresh tokens to be issued to JavaScript apps as well. This ends up being a policy decision of the authorization server as to whether refresh tokens will be issued, depending on the level of risk the authorization server is willing to tolerate.

Additionally, the additions of browser APIs such as `ServiceWorkers` means that now browser-based apps have the potential to run code when the user isn’t actively using the browser, such as in response to a Background Sync event. This means there is now more potential use for refresh tokens in browser apps.

If the authorization server wishes to allow JavaScript apps to use refresh tokens, then they must also follow the best practices outlined in “OAuth 2.0 Security Best Current Practice” and “OAuth 2.0 for Browser-Based Apps“, two recent documents adopted by the OAuth Working Group. Specifically, refresh tokens must be valid for only one use, and the authorization server must issue a new refresh token each time a new access token is issued in response to a refresh token grant. This provides the authorization server a way to detect if a refresh token has been copied and used by an attacker, since in normal operation of an app a refresh token would be used only once.

Refresh tokens must also either have a set maximum lifetime, or expire if they are not used within some amount of time. This is again another way to help mitigate the risks of a stolen refresh token.

## Storing Tokens

The browser-based app will need to temporarily store some pieces of information during the authorization flow, and then permanently store the resulting access token and refresh token. This provides some challenges in the browser environment since there are currently no general-purpose secure storage mechanism in browsers.

Generally, the browser’s `LocalStorage` API is the best place to store this data as it provides the easiest API to store and retrieve data and is about as secure as you can get in a browser. The downside is that any scripts on the page, even from different domains such as your analytics or ad network, will be able to access the `LocalStorage` of your application. This means anything you store in `LocalStorage` is potentially visible to third-party scripts on your page.

Because of the risks of data leakage from third-party scripts, it is extremely important to have a good Content-Security Policy configured for your app so that you can be more confident that arbitrary scripts aren’t able to run in the application. A good document on configuring a Content Security Policy is available from OWASP at https://owasp.org/www-project-cheat-sheets/cheatsheets/Content_Security_Policy_Cheat_Sheet.html

## Choosing an Alternative Architecture

Due to the inherent risks of performing an OAuth flow in a pure JavaScript environment, as well as the risks of storing tokens in a JavaScript app, it is also advisable to consider an alternative architecture where the OAuth flow is handled outside of the JavaScript code by a dynamic backend component. This is a relatively common architectural pattern where an application is served from a dynamic backend such as a .NET or Java app, but it uses a single-page app framework like React or Angular for its UI. If your app falls under this architectural pattern, then the best option is to move all of the OAuth flow to the server component, and keep the access tokens and refresh tokens out of the browser entirely. Note that in this case since your app has a dynamic backend, it is also considered a confidential client and can use a client secret to further protect the OAuth exchanges.

This pattern is described in more detail in “OAuth 2.0 for Browser-Based Apps“.

[Previous](https:// "Previous")
/
[Next](https:// "Next")
