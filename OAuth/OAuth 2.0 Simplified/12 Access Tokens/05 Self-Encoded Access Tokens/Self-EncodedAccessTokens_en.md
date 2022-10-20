<h1 align="center">Self-Encoded Access Tokens</h1>

Self-encoded tokens provide a way to avoid storing tokens in a database by encoding all of the necessary information in the token string itself. The main benefit of this is that API servers are able to verify access tokens without doing a database lookup on every API request, making the API much more easily scalable.

The benefit of OAuth 2.0 Bearer Tokens is that applications don’t need to be aware of how you’ve decided to implement access tokens in your service. This means it’s possible to change your implementation later without affecting clients.

If you already have a distributed database system that is horizontally scalable, then you may not gain any benefits by using self-encoded tokens. In fact, using self-encoded tokens if you’ve already solved the distributed database problem will only introduce new issues, as invalidating self-encoded tokens becomes an additional hurdle.

There are many ways to self-encode tokens. The actual method you choose is only important to your implementation, since the token information is not exposed to external developers.

The most common way to implement self-encoded tokens is to use the JWS spec, creating a JSON-serialized representation of all the data you want to include in the token, and signing the resulting string with a private key known only to your authorization server.

RFC 9068 defines a standard way to use JWTs as access tokens, based on the real-world deployment experience of a number of large OAuth providers. This spec defines a data structure to use when including claims about authentication, authorization, and identity. See https://oauth.net/2/jwt-access-tokens/ for further details.

## JWT Access Token Encoding

The code below is written in PHP and uses the Firebase PHP-JWT library to encode and verify tokens. You’ll need to include that library in order to run the sample code

In practice, the authorization server will have a private key it uses for signing tokens, and the resource server would fetch the public key from the authorization server metadata to use to validate the tokens. In this example we generate a new private key each time and validate tokens in the same script. In reality you’d need to store the private key somewhere to use the same key to sign tokens consistently.

```
<?php
use \Firebase\JWT\JWT;

# Generate a private key to sign the token.
# The public key would need to be published at the authorization
# server if a separate resource server needs to validate the JWT

$private_key = openssl_pkey_new([
  'digest_alg' => 'sha256',
  'private_key_bits' => 1024,
  'private_key_type' => OPENSSL_KEYTYPE_RSA
]);

# Set the user ID of the user this token is for
$user_id = 1000;

# Set the client ID of the app that is generating this token
$client_id = 'https://example-app.com';

# Provide the list of scopes this token is valid for
$scope = 'read write';

$token_data = array(

  # Issuer (the authorization server identifier)
  'iss' => 'https://' . $_SERVER['PHP_SELF'],

  # Expires At
  'exp' => time()+7200, // Valid for 2 hours

  # Audience (The identifier of the resource server)
  'aud' => 'api://default',

  # Subject (The user ID)
  'sub' => $user_id,

  # Client ID
  'client_id' => $client_id,

  # Issued At
  'iat' => time(),

  # Identifier of this token
  'jti' => microtime(true).'.'.bin2hex(random_bytes(10)),

  # The list of OAuth scopes this token includes
  'scope' => $scope
);
$token_string = JWT::encode($token_data, $private_key, 'RS256');
```

This will result in a string such as:

```
eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiJ9.eyJpc3MiOiJodH
RwczpcL1wvYXV0aG9yaXphdGlvbi1zZXJ2ZXIuY29tXC8iLCJle
HAiOjE2MzczNDQ1NzIsImF1ZCI6ImFwaTpcL1wvZGVmYXVsdCIs
InN1YiI6MTAwMCwiY2xpZW50X2lkIjoiaHR0cHM6XC9cL2V4YW1
wbGUtYXBwLmNvbSIsImlhdCI6MTYzNzMzNzM3MiwianRpIjoiMT
YzNzMzNzM3Mi4yMDUxLjYyMGY1YTNkYzBlYmFhMDk3MzEyIiwic
2NvcGUiOiJyZWFkIHdyaXRlIn0.SKDO_Gu96WeHkR_Tv0d8gFQN
1SEdpN8S_h0IJQyl_5syvpIRA5wno0VDFi34k5jbnaY5WHn6Y91
2IOmg6tMO91KlYOU1MNdVhHUoPoNUzYtl_nNab7Ywe29kxgrekm
-67ZInDI8RHbSkL7Z_N9eZz_J8c3EolcsoIf-Dd5n9y_Y
```

This token is made up of three components, separated by periods. The first part describes the signature method used. The second part contains the token data. The third part is the signature.

For example, this token’s first component is this JSON object:

```
{
   "typ":"JWT",
   "alg":"RS256”
 }
```

The second component contains the actual data the API endpoint needs in order to process the request, such as user identification and scope access.

```
{
  "iss": "https://authorization-server.com/",
  "exp": 1637344572,
  "aud": "api://default",
  "sub": 1000,
  "client_id": "https://example-app.com",
  "iat": 1637337372,
  "jti": "1637337372.2051.620f5a3dc0ebaa097312",
  "scope": "read write"
}
```

The two components are then base64-encoded, and the JWT library calculates the RS256 signature of the two strings, then joins all three parts with a period:

```
eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiJ9.eyJpc3MiOiJodH
RwczpcL1wvYXV0aG9yaXphdGlvbi1zZXJ2ZXIuY29tXC8iLCJle
HAiOjE2MzczNDQ1NzIsImF1ZCI6ImFwaTpcL1wvZGVmYXVsdCIs
InN1YiI6MTAwMCwiY2xpZW50X2lkIjoiaHR0cHM6XC9cL2V4YW1
wbGUtYXBwLmNvbSIsImlhdCI6MTYzNzMzNzM3MiwianRpIjoiMT
YzNzMzNzM3Mi4yMDUxLjYyMGY1YTNkYzBlYmFhMDk3MzEyIiwic
2NvcGUiOiJyZWFkIHdyaXRlIn0.SKDO_Gu96WeHkR_Tv0d8gFQN
1SEdpN8S_h0IJQyl_5syvpIRA5wno0VDFi34k5jbnaY5WHn6Y91
2IOmg6tMO91KlYOU1MNdVhHUoPoNUzYtl_nNab7Ywe29kxgrekm
-67ZInDI8RHbSkL7Z_N9eZz_J8c3EolcsoIf-Dd5n9y_Y
```

## Decoding

Verifying the access token can be done by using the same JWT library. The library will decode and verify the signature at the same time, and throws an exception if the signature was invalid, or if the expiration date of the token has already passed.

You’ll need the public key corresponding to the private key that signed the token. Typically you can fetch this from the authorization server’s metadata document, but in this example we will derive the public key from the private key generated earlier.

Note: Anyone can read the token information by base64-decoding the middle section of the token string. For this reason, it’s important that you do not store private information or information you do not want a user or developer to see in the token. If you want to hide the token information, you can use the JSON Web Encryption spec to encrypt the data in the token.

```
$public_key = openssl_pkey_get_details($private_key)['key'];

try {
  # Note: You must provide the list of supported algorithms in order to prevent
  # an attacker from bypassing the signature verification. See:
  # https://auth0.com/blog/critical-vulnerabilities-in-json-web-token-libraries/
  $token = JWT::decode($token_string, $jwt_key, ['RS256']);
  $error = false;
} catch(\Firebase\JWT\ExpiredException $e) {
  $token = false;
  $error = 'expired';
  $error_description = 'The token has expired';
} catch(\Firebase\JWT\SignatureInvalidException $e) {
  $token = false;
  $error = 'invalid';
  $error_description = 'The token provided was malformed';
} catch(Exception $e) {
  $token = false;
  $error = 'unauthorized';
  $error_description = $e->getMessage();
}

if($error) {
  header('HTTP/1.1 401 Unauthorized');
  echo json_encode(array(
    'error'=>$error,
    'error_description'=>$error_description
  ));
  die();
} else {
  // Now $token has all the data that we encoded in it originally
  print_r($token);
}
```

At this point, the service has all the information it needs such as the user ID, scope, etc, available to it, and didn’t have to do a database lookup. Next it can check to make sure the access token hasn’t expired, can verify the scope is sufficient to perform the requested operation, and can then process the request.

## Invalidating

Because the token can be verified without doing a database lookup, there is no way to invalidate a token until it expires. You’ll need to take additional steps to invalidate tokens that are self-encoded, such as temporarily storing a list of revoked tokens, which is one use of the jti claim in the token. See Refreshing Access Tokens for more information.

[Previous](https:// "Previous")
/
[Next](https:// "Next")
