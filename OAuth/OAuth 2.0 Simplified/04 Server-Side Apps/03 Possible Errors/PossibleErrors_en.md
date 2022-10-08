<h1 align="center">Possible Errors</h1>

There are several cases where you may get an error response during authorization.

Errors are indicated by redirecting back to the provided redirect URL with additional parameters in the query string. There will always be an error parameter, and the redirect may also include `error_description` and `error_uri`.

For example,

```
https://example-app.com/cb?error=invalid_scope
```

Despite the fact that servers return an `error_description` key, the error description is not intended to be displayed to the user. Instead, you should present the user with your own error message. This allows you to tell the user an appropriate action to take to correct the problem, and also gives you a chance to localize the error messages if you’re building a multi-language website.

## Invalid redirect URL

If the redirect URL provided is invalid, the authorization server will not redirect to it. Instead, it may display a message to the user describing the problem instead.

## Unrecognized client_id

If the client ID is not recognized, the authorization server will not redirect the user. Instead, it may display a message describing the problem.

## The user denies the request

If the user denies the authorization request, the server will redirect the user back to the redirect URL with `error=access_denied` in the query string, and no code will be present. It is up to the app to decide what to display to the user at this point.

## Invalid parameters

If one or more parameters are invalid, such as a required value is missing, or the `response_type` parameter is wrong, the server will redirect to the redirect URL and include query string parameters describing the problem. The other possible values for the error parameter are:

#### invalid_request:

The request is missing a required parameter, includes an invalid parameter value, or is otherwise malformed.

#### unauthorized_client:

The client is not authorized to request an authorization code using this method.

#### unsupported_response_type:

The authorization server does not support obtaining an authorization code using this method.

#### invalid_scope:

The requested scope is invalid, unknown, or malformed.

#### server_error:

The authorization server encountered an unexpected condition which prevented it from fulfilling the request.

#### temporarily_unavailable:

The authorization server is currently unable to handle the request due to a temporary overloading or maintenance of the server.

In addition, the server may include parameters `error_description` and `error_uri` with additional information about the error.

[Previous](https://github.com/alithecodeguy/articles/blob/main/OAuth/OAuth%202.0%20Simplified/04%20Server-Side%20Apps/02%20Example%20Flow/ExampleFlow_en.md "Previous")
/
[Next](https://github.com/alithecodeguy/articles/blob/main/OAuth/OAuth%202.0%20Simplified/04%20Server-Side%20Apps/04%20User%20Experience%20Considerations%20and%20Security%20Considerations/UserExperienceConsiderations_en.md "Next")
