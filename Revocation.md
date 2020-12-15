Navigation | [[Table Of Contents]](README.md#navigating-the-documentation)

TrustFabric Token Revocation
============================

### Token Revocation

Token revocation is supported via the `/revoke` endpoint as described in [RFC 7009](https://tools.ietf.org/html/rfc7009).  Token revocation endpoint should be restricted to TrustFabric administrators or operators who have admin roles on TrustFabric Authorization Server. Since the tokens issued by TrustFabric are transparent tokens, the revoked tokens may not be blocked from usage immediately. Therefore, it is required to limit the lifetime of the tokens to as short a duration as possible. 

### Revocation Token Hierarchy

TrustFabric supports revocation of Authorization code grants, Access tokens, Refresh tokens and Identity tokens. Any of these tokens can be revoked via  `/revoke` endpoint along with the appropriate token hint. 

* If an access token or identity token is revoked, any further usage of that token will be blocked. 
* If a refresh token is revoked, any access token and refresh token issued from it will be blocked. 
* If a code grant is revoked, any access token, refresh token and identity token issued from it will be blocked. 

To support this hierarchical revocation, TrustFabric tokens should be linked to its parent token using the `jti
` claim. A new claim `parent_jti` is added to the access token, refresh token and identity token. If a revoked token's
 `jti` is present in the current tokens `jti` or `parent_jti` claim, then the token should be rejected. 

### Revocation Callback Service

To support immediate revocation of the transparent tokens, TrustFabric supports Token Revocation Callback Service. Applications can register a  callback URL to this endpoint. When a token is revoked, any audience in the token with a registered callback will be notified.  In case of a code grant, or a refresh token, as the audience list does not contain client applications, TrustFabric will fetch the actual audience from its datastore and notify client applications. Applications can register for the revocation list call as below:

```http
    POST /register-revocation-callback HTTP/1.1
    Host: server.acme.org
    Authorization: Bearer <JWT Access Token>
    Content-Type: application/x-www-form-urlencoded

    url%3Dhttps%3A%2F%2Fclientapp.acme.org%2Frevocation-callback%0A
```

Response:

```http
    HTTP/1.1 200 OK
    Content-Type: application/json;

    {
        "expires_in":3600
    }
```

The registered callback URL is valid only for the duration returned in expires_in field. Applications must register the callback again if the time is past the value specified in expires_in.

When a token is revoked, the callback URL is invoked as below:

```http
    GET /revocation-callback?jti=xxxx,jti=yyyy,jti=zzzzz
    Host: clientapp.acme.org
    Authorization: Bearer <Tustfabric jwt token>
```

To prevent misuse of callback URLs, applications registering the callback URL must verify the Bearer token sent 
during the callback, and validate that the issuer is TrustFabric.


