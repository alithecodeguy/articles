<h1 align="center">Checklist for Server Support for Native Apps</h1>

To summarize this chapter, your authorization server should support the following in order to fully support secure authorization for native apps.

- Allow clients to register custom URL schemes for their redirect URLs.
- Support loopback IP redirect URLs with arbitrary port numbers in order to support desktop apps.
- Don’t assume native apps can keep a secret. Require all apps to declare whether they are public or confidential, and only issue client secrets to confidential apps.
- Support the PKCE extension, and require that public clients use it.
- Attempt to detect when the authorization interface is embedded in a native app’s web view, instead of launched in a system browser, and reject those requests.

[Previous](https:// "Previous")
/
[Next](https:// "Next")
