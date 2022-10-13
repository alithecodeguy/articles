<h1 align="center">Defining Scopes</h1>

Scope is a mechanism to let an application request limited access to a user’s data.

The challenge when defining scopes for your service is to not get carried away with defining too many scopes. Users need to be able to understand what level of access they are granting to the application, and this will be presented to the user in some sort of list. When presented to the user, they need to actually understand what is going on and not get overwhelmed with information. If you over-complicate it for users, they will just click “ok” until the app works, and ignore any warnings.

## Read vs. Write

Read vs write access is a good place to start when defining scopes for a service. Typically read access to a user’s private profile information is treated with separate access control from apps wanting to update the profile information.

Apps that need to be able to create content on behalf of a user (for example, third-party Twitter apps that post tweets to a user’s timeline) need a different level of access from apps that only need to read a user’s public data.

## Restricting Access to Sensitive Information

Often a service will have various aspects of a user account that have different levels of security. For example, GitHub has a separate scope that allows applications to have access to private repos. By default, applications don’t have access to private repos unless they ask for that scope, so users can feel comfortable knowing that only apps they choose can access their private repos belonging to their organization.

GitHub provides a separate scope that allows applications to delete repos, so users can rest assured that random applications can’t go around deleting their repos.

Dropbox provides a way for applications to restrict themselves to only be able to edit files in a single folder. This provides a way that users can try out apps that use Dropbox as a storage or syncing mechanism without worrying about the application potentially having the ability to read all their files.

## Selectively Enabling Access by Functionality

A great use of scope is to selectively enable access to a user’s account based on the functionality needed. For example, Google offers a set of scopes for their various services such as Google Drive, Gmail, YouTube, etc. This means applications that need to access the YouTube API won’t necessarily also be able to access the user’s Gmail account.

Google’s API is a great example of effectively using scope. For a full list of the scopes that the Google OAuth API supports, visit their OAuth 2.0 Playground at https://developers.google.com/oauthplayground/

## Limiting Access to Billable Resources

If your service provides an API that may cause the user to incur charges, scope is a great way to protect against applications abusing this.

Let’s use an example of a service that provides advanced capabilities that use licensed content, in this case one that provides an API that aggregates demographic data for a given area. The user racks up charges as the service is used, and the cost is based on the size of the area being queried. A user signing in to an app that uses a completely different part of the API would want to ensure this app is not able to use the demographics API, since that would cause that user to incur charges. The service should in this case define a special scope, say, “demographics”. The demographics API should only respond to API requests from tokens that contain this scope.

In this example, the demographics API could use the token introspection endpoint to look up the list of scopes that are valid for this token. If the response does not include “demographics” in the list of scopes, the endpoint would reject the request with an HTTP 403 response.

[Previous](https:// "Previous")
/
[Next](https:// "Next")
