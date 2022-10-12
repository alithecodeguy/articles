<h1 align="center">Security Considerations</h1>

Below are some known issues that should be taken into consideration when building an authorization server.

In addition to the considerations listed here, there is more information available in the OAuth 2.0 Thread Model and Security Considerations RFC as well as OAuth 2.0 Security Best Current Practice.

## Phishing Attacks

One potential attack against OAuth servers is a phishing attack. This is where an attacker makes a web page that looks identical to the service’s authorization page, which typically contain username and password fields. Then, through various means, the attacker can trick the user in to visiting the page. Unless the user can inspect the address bar of the browser, the page may look otherwise identical to the genuine authorization page, and the user may enter their username and password.

One way attackers can attempt to trick the user into visiting the counterfeit server is by embedding this phishing page in an embedded web view in a native application. Since embedded web views don’t show the address bar, the user then has no way to visually confirm they are on the legitimate site. This is unfortunately common in mobile applications, and often justified by the developer wanting to provide a better user experience by keeping the user in the application through the entire login process. Some OAuth providers encourage third party applications to either open a web browser or launch the provider’s native application instead of allowing them to embed an authorization page in a web view.

## Countermeasures

Ensure the authorization server is served via https to avoid DNS spoofing.

The authorization server should educate developers of the risks of phishing attacks, and can take steps to prevent the page from being embedded in native applications or in iframes.

Users should be educated about the dangers of phishing attacks, and should be taught best practices such as only accessing applications that they trust, and periodically reviewing the list of applications they’ve authorized to revoke access to apps they no longer use.

The service may want to validate third-party applications prior to allowing other users to use the application. Services such as Instagram and Dropbox currently do this, where upon initial creation of an application, the app is only usable by the developer or other whitelisted user accounts. After the app is submitted for approval and reviewed, then it can be used by the whole user base of the service. This gives the service a chance to inspect how the application interacts with the service.

## Clickjacking

In a clickjacking attack, the attacker creates a malicious website in which it loads the authorization server URL in a transparent iframe above the attacker’s web page. The attacker’s web page is stacked below the iframe, and has some innocuous-looking buttons or links, placed very carefully to be directly under the authorization server’s confirmation button. When the user clicks the misleading visible button, they are actually clicking the invisible button on the authorization page, thereby granting access to the attacker’s application. This allows the attacker to trick the user into granting access without their knowledge.

## Countermeasures

This kind of attack can be prevented by ensuring the authorization URL is always loaded directly in a native browser, and not embedded in an iframe. Newer browsers have the ability for the authorization server to set an HTTP header, X-Frame-Options, and older browsers can use common Javascript “frame-busting” techniques.

## Redirect URL Manipulation

An attacker can construct an authorization URL using a client ID that belongs to a known good application, but set the redirect URL to a URL under the control of the attacker. If the authorization server does not validate redirect URLs, and the attacker uses the “token” response type, the user will be returned to the attacker’s application with the access token in the URL. If the client is a public client, and the attacker intercepts the authorization code, then the attacker can also exchange the code for an access token.

Another similar attack is when the attacker can spoof the user’s DNS, and the registered redirect is not an https URL. This would allow the attacker to pretend to be the valid redirect URL, and steal the access token that way.

The “Open Redirect” attack is when the authorization server does not require an exact match of the redirect URL, and instead allows an attacker to construct a URL that will redirect to the attacker’s website. Whether or not this ends up being used to steal authorization codes or access tokens, this is also a danger in that it can be used to launch other unrelated attacks. More details about the Open Redirect attack can be found at https://oauth.net/advisories/2014-1-covert-redirect/.

## Countermeasures

The authorization server must require that one or more redirect URLs are registered by the application, and only redirect to an exact match of a previously registered URL.

The authorization server should also require that all redirect URLs are https. Since this can sometimes be a burden during development, it is also acceptable to allow non-https redirect URLs while the application is “in development” and can only be accessed by the developer, and then require that the redirect URL is changed to an https URL before the application is published and available to other users.

[Previous](https:// "Previous")
/
[Next](https:// "Next")
