<h1 align="center">PKCE Extension</h1>

Since redirect URLs on native platforms have limited ability to be enforced, there is another technique for gaining additional security called Proof Key for Code Exchange, or PKCE for short, pronounced “pixie”.

This technique involves the native app creating an initial random secret, and using that secret again when exchanging the authorization code for an access token. This way, if another app intercepts the authorization code, it will be unusable without the original secret.

Note that PKCE doesn’t prevent app impersonation, it only prevents authorization codes from being used by a different app than the one that started the flow.

See Proof Key for Code Exchange for details.

[Previous](https:// "Previous")
/
[Next](https:// "Next")
