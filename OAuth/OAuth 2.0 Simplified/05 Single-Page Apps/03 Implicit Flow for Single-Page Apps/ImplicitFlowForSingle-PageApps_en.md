<h1 align="center">Implicit Flow for Single-Page Apps</h1>

Some services have historically used the alternative Implicit Flow for single-page apps, rather than the current recommendation of using the Authorization Code with PKCE.

The Implicit Flow bypasses the code exchange step, and instead the access token is returned in the query string fragment to the client immediately.

There are a number of concerns with this approach, enough that many providers have opted to avoid implementing the Implicit flow completely.

The Implicit flow in OAuth 2.0 was created over 10 years ago, when browsers worked very differently than they do today. The primary reason the Implicit flow was created was because of an old limitation in browsers. It used to be the case that JavaScript could only make requests to the same domain that the page was loaded from. However, the standard OAuth Authorization Code flow requires that a POST request is made to the OAuth server’s token endpoint, which is often on a different domain than the app. That meant there was previously no way to use this flow from JavaScript. The Implicit flow worked around this limitation by avoiding that POST request, and instead returning the access token immediately in the redirect.

Today, Cross-Origin Resource Sharing (CORS) is universally adopted by browsers, removing the need for this compromise. CORS provides a way for JavaScript to make requests to servers on a different domain as long as the destination allows it. This opens up the possibility of using the Authorization Code flow in JavaScript.

It’s worth noting that the Implicit flow has always been seen as a compromise compared to the Authorization Code flow. For example, the spec provides no mechanism to return a refresh token in the Implicit flow, as it was seen as too insecure to allow that. The spec also recommends short lifetimes and limited scope for access tokens issued via the Implicit flow.

In any case, with both the Implicit Flow as well as the Authorization Code Flow with PKCE, the server must require registration of the redirect URL in order to maintain the security of the flow.

[Previous](https:// "Previous")
/
[Next](https:// "Next")
