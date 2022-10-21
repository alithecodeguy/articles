<h1 align="center">Access Token Lifetime</h1>

When your service issues access tokens, you’ll need to make some decisions as to how long you want the tokens to last. Unfortunately there is no blanket solution for every service. There are various tradeoffs that come with the different options, so you should choose the option (or combination of options) that best suit your application’s need.

## Short-lived access tokens and long-lived refresh tokens

A common method of granting tokens is to use a combination of access tokens and refresh tokens for maximum security and flexibility. The OAuth 2.0 spec recommends this option, and several of the larger implementations have gone with this approach.

Typically services using this method will issue access tokens that last anywhere from several hours to a couple weeks. When the service issues the access token, it also generates a refresh token that never expires and returns that in the response as well. (Note that refresh tokens can’t be issued using the Implicit grant.)

When the access token expires, the application can use the refresh token to obtain a new access token. It can do this behind the scenes, and without the user’s involvement, so that it’s a seamless process to the user.

The main benefit of this approach is that the service can use self-encoded access tokens which can be verified without a database lookup. However, this means there is no way to expire those tokens directly, so instead, the tokens are issued with a short expiration time so that the application is forced to continually refresh them, giving the service a chance to revoke an application’s access if needed.

From the third-party developer’s perspective, it is often frustrating to have to deal with refresh tokens. Developers strongly prefer access tokens that don’t expire, since it’s much less code to deal with. In order to help mitigate these concerns, services will often build the token refreshing logic into their SDK, so that the process is transparent to developers.

In summary, use short-lived access tokens and long-lived refresh tokens when:

- you want to use self-encoded access tokens
- you want to limit the risk of leaked access tokens
- you will be providing SDKs that can handle the refresh logic transparently to developers

## Short-lived access tokens and no refresh tokens

If you want to ensure users are aware of applications that are accessing their account, the service can issue relatively short-lived access tokens without refresh tokens. The access tokens may last anywhere from the current application session to a couple weeks. When the access token expires, the application will be forced to make the user sign in again, so that you as the service know the user is continually involved in re-authorizing the application.

Typically this option is used by services where there is a high risk of damage if a third-party application were to accidentally or maliciously leak access tokens. By requiring that users are constantly re-authorizing the application, the service can ensure that potential damage is limited if an attacker were to steal access tokens from the service.

By not issuing refresh tokens, this makes it impossible to applications to use the access token on an ongoing basis without the user in front of the screen. Applications that need access in order to continually sync data will be unable to do so under this method.

From the user’s perspective, this is the option most likely to frustrate people, since it will look like the user has to continually re-authorize the application.

In summary, use short-lived access tokens with no refresh tokens when:

- you want to the most protection against the risk of leaked access tokens
- you want to force users to be aware of third-party access they are granting
- you don’t want third-party apps to have offline access to users’ data

## Non-expiring access tokens

Non-expiring access tokens are the easiest method for developers. If you choose this option, it is important to consider the trade-offs you are making.

It isn’t practical to use self-encoded tokens if you want to be able to revoke them arbitrarily. As such, you’ll need to store these tokens in some sort of database, so they can be deleted or marked as invalid as needed.

Note that even if the service intends on issuing non-expiring access tokens for normal use, you’ll still need to provide a mechanism to expire them under exceptional circumstances, such as if the user explicitly wants to revoke an application’s access, or if a user account is deleted.

Non-expiring access tokens are much easier for developers testing their own applications. You can even pre-generate one or more non-expiring access tokens for developers and show it to them on the application details screen. This way they can immediately start making API requests with the token, and not worry about setting up an OAuth flow in order to start testing your API.

In summary, use non-expiring access tokens when:

- you have a mechanism to revoke access tokens arbitrarily
- you don’t have a huge risk if tokens are leaked
- you want to provide an easy authentication mechanism to your developers
- you want third-party applications to have offline access to users’ data

[Previous](https:// "Previous")
/
[Next](https:// "Next")
