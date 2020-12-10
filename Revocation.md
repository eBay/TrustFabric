TrustFabric Token Revocation
============================

### Token Revocation:

Token revocation is supported via the `/revoke` endpoint as described in **[RFC 7009](https://tools.ietf.org/html/rfc7009)**.  Token revocation endpoint should be restricted to Trustfabric administrators or operators which has admin roles on Trustfabric Authorization Server. Since the tokens issued by Trustfabric are transparent tokens, the revoked tokens may not be blocked from usage immediately. Therefore it is requried to limit the lifetime of the tokens to shorter duration as possible. 

### Revocation Token Hierarchy:

Trustfabric supports revocation of Authorization code grants, access token, refresh token and identity tokens. Any of these tokens can be passed to the `/revoke` endpoint along with the appropriate token hint. If an access token or identity token is invoked the further usage of that token will be blocked. If a refresh token is blocked, any access token and refresh token issued from it will be blocked. If a code grant is revoked, any access token, refresh token and identity token issued from it will be blocked. 

To support this hierarchial revocation, Trustfabric tokens should be linked to its parent token using the `jti` claim. A new cliam 'parent_jti' is added to the access token, refresh token and identity token. If a revoked token's `jti` is present in the current tokens `jti` or `parent_jti` claim, that token should be rejected. 

### Revocation Callback Service:

To support immediate revocation of the transparent tokens, Trustfabric supports Token Revocation Callback Service. Applications can register their callback URL to this endpoint. When a token is revoked, Trustfabric invokes the registered callbacks if the audience claim of the affected token contains the applications that registered their callback.  If the revoked token is a code grant or refresh token the audience list in the token does not correspond to the client applicatins. Under such scenario, Trustfabric fetches the actual audiences from its datastore. Applications can register for the revocation list call as below:

```http
    POST /register-revocation-callback HTTP/1.1
    Host: server.acme.org
    Authorization: Bearer <JWT Access Token>
    Content-Type: application/x-www-form-urlencoded

    url%3Dhttps%3A%2F%2Fclientapp.acme.org%2Frevocation-callback%0A
```

The server responds with:

```http
    HTTP/1.1 200 OK
    Content-Type: application/json;

    {
        "expires_in":3600
    }
```

The registered callback url is valid only for the duration returned in expires_in field. Applications must register the callback again if the time is past the value specified in expires_in.

When a token is revoked, the callback url is invoked as below:

```http
    GET /revocation-callback?jti=xxxx,jti=yyyy,jti=zzzzz
    Host: clientapp.acme.org
    Authorization: Bearer <Tustfabric jwt token>
```

To prevent misuse of callback url's applications registering the callback url must verify the Bearer token sent 
during the callback and make sure it is coming from the Trustfabric service.


