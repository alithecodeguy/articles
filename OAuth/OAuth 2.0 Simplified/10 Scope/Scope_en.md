<h1 align="center">Scope</h1>

Scope is a way to limit an app’s access to a user’s data. Rather than granting complete access to a user’s account, it is often useful to give apps a way to request a more limited scope of what they are allowed to do on behalf of a user.

Some apps only use OAuth in order to identify the user, so they only need access to a user ID and basic profile information. Other apps may need to know more sensitive information such as the user’s birthday, or they may need the ability to post content on behalf of the user, or modify profile data. Users will be more willing to authorize an application if they know exactly what the application can and cannot do with their account. Scope is a way to control access and help the user identify the permissions they are granting to the application.

It’s important to remember that scope is not the same as the internal permissions system of an API. Scope is a way to limit what an application can do within the context of what a user can do. For example, if you have a user in the “customer” group, and the application is requesting the “admin” scope, the OAuth server is not going to create an access token with the “admin” scope, because that user is not allowed to use that scope themselves.

Scope should be thought of as the application requesting permission from the user who’s using the app.

- Defining Scopes
- User Interface
- Checkboxes

[Previous](https:// "Previous")
/
[Next](https:// "Next")
