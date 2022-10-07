<h1 align="center">Mobile and Native Apps</h1>

Like single-page apps, mobile apps also cannot maintain the confidentiality of a client secret. Because of this, mobile apps must also use an OAuth flow that does not require a client secret. The current best practice is to use the Authorization Flow with PKCE, along with launching an external browser, in order to ensure the native app cannot modify the browser window or inspect the contents.

Many websites provide mobile SDKs which handle the authorization process for you. For those services, you are probably better off using their SDKs directly, since they may have augmented their APIs with non-standard additions. Google provides an open source library called AppAuth which handles the implementation details of the flow described below. It is meant to be able to work with any OAuth 2.0 server that implements the spec. In the case that the service does not a provide their own abstraction, and you have to use their OAuth 2.0 endpoints directly, this section describes how to use the authorization code flow with PKCE to interface with an API.

- Authorization
- Security Considerations

[Previous](https: "Previous")
/
[Next](https: "Next")
