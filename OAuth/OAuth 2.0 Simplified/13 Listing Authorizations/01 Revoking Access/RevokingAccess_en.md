<h1 align="center">Revoking Access</h1>

There are a few reasons you might need to revoke an application’s access to a user’s account.

- The user explicitly wishes to revoke the application’s access, such as if they’ve found an application they no longer want to use listed on their authorizations page
- The developer wants to revoke all user tokens for their application
- The developer deleted their application
- You as the service provider have determined an application is compromised or malicious, and want to disable it

Depending on how you’ve implemented generating access tokens, revoking them will work in different ways.

## Token Database

If you store access tokens in a database, then it is relatively easy to revoke all tokens that belong to a particular user. You can easily write a query that finds and deletes tokens belonging to the user, such as looking in the token table for their `user_id`. Assuming your resource server validates access tokens by looking them up in the database, then the next time the revoked client makes a request, their token will fail to validate.

Self-Encoded Tokens
If you have a truly stateless mechanism of verifying tokens, and your resource server is validating tokens without sharing information with another system, then the only option is to wait for all outstanding tokens to expire, and prevent the application from being able to generate new tokens for that user by blocking any refresh token requests from that client ID. This is the primary reason to use extremely short-lived tokens when you are using self-encoded tokens.

If you can afford some level of statefulness, you could push a revocation list of token identifiers to your resource servers, and your resource servers can check that list when validating a token. The access token can contain a unique ID (e.g. the `jti` claim) which can be used to keep track of individual tokens. If you want to revoke a particular token, you would need to put that token’s `jti` into a list somewhere that can be checked by your resource servers. Of course this means your resource servers are no longer doing a purely stateless check, so this may not be an option available for every situation.

You will also need to invalidate the application’s refresh tokens that were issued along with an access token. Revoking the refresh token means the next time the application attempts to refresh the access token, the request for a new access token will be denied.

[Previous](https:// "Previous")
/
[Next](https:// "Next")
