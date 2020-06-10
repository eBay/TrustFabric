TrustFabric Tokens
==================
Application Tokens
------------------
TrustFabric implements three tokens for applications:
1. Bootstrap token (a.k.a. Referral tokens)
1. Access token
1. Refresh tokens

Besides tokens, specification supports X.509 certificates for TLS termination and network identity of server. Specification also supports Kerberos tokens for authentication.

### Bootstrap token
Bootstrap tokens (a.k.a. Referral tokens) are equivalent to OAuth2 code-grant. These are referral tokens issued to application by a trusted referrer. These are implemented as JWT tokens and must contain aud (application) and sub (referrer), besides standard JWT claims.
```json
{
    "alg": "RS256",
    "typ": "JWT",
    "kid": "41d3c61d",
    "x5u": "https://as.acme.org/v2/k/41d3c61d"
}
{
  "exp": 1591742754,
  "iat": 1591739154,
  "iss": "https://as.acme.org/v2/",
  "nbf": 1591739154,
  "jti": "05612104",
  "sub": "uid=jdoe, ou=platform, o=people, dc=acme, dc=org",
  "aud": "cn=prod-cart + L=production, ou=cartdb, o=cart, dc=acme, dc=org",
  "inst": [
    "10.147.185.209"
  ]
  ...
}
```
In the token above `iss` represents the issuer, `sub` represents the referrer identity subject (LDAP DN used here), `aud` represent the audience for the referral token i.e. application receiving the bootstrap token (represented using LDAP DN). The instance claim `inst` is an array of IP addresses to which this token is issued. These addresses should represent the network identity of the application (`aud`). Claim can be used to perform the trust verification via reverse lookup for the `token` flow and can also be used to do origin validation during token verification.

### Access token
```json
{
    "alg": "RS256",
    "typ": "JWT",
    "kid": "41d3c61d",
    "x5u": "https://as.acme.org/v2/k/41d3c61d"
}
{
  "exp": 1591842754,
  "iat": 1591739154,
  "iss": "https://as.acme.org/v2/",
  "nbf": 1591739154,
  "jti": "7630F317",
  "sub": "cn=prod-cart + L=production, ou=cartdb, o=cart, dc=acme, dc=org",
  "inst": [
    "10.147.185.209"
  ]
  ...
}
```
### Refresh token

```json
{
    "alg": "RS256",
    "typ": "JWT",
    "kid": "41d3c61d",
    "x5u": "https://as.acme.org/v2/k/41d3c61d"
}
{
  "exp": 1591842754,
  "iat": 1591739154,
  "iss": "https://as.acme.org/v2/",
  "nbf": 1591739154,
  "jti": "B766E832",
  "sub": "cn=prod-cart + L=production, ou=cartdb, o=cart, dc=acme, dc=org",
  "inst": [
    "10.147.185.209"
  ]
  ...
}
```

Obtaining Application Tokens
----------------------------
Applications share identity with other applications on the network for
app-app interactions. Application needs to be bootstrapped with identity
for these interactions. TrustFabric is a password-less mechanism and
implements a two step mechanism to establish identity.

-   Referral identity injection using bootstrap token (by trusted referrer)

-   Obtaining Identity and Access tokens for interactions using referral identity (bootstrap tokens)

Application needs to be bootstrapped with referral identity token (A.K.A code grant)

Bootstrapping Application Identity
----------------------------------
As part of referral identity injection, application deployment is
provided with a short duration referral identity token by TrustFabric, based on trust
declared by trusted referrer e.g. DevOps managing applications or k8s
operators etc. This referral identity can later be used by application
to obtain identity and access tokens. TrustFabric leverages OAuth2 to
implement token request flows. It is important to note that unlike OAuth2, the token interaction produces access tokens with application's identity and not the tokens for delegated access. Identity, subject and access claims embedded in the token are for the application. Unlike OIDC, there are no separate identity tokens, access token is implemented using JWT as transparent token and can be used as identity token (has identity claims embedded).The referral tokens are treated as
code-grant issued by DevOps or by Operators. Code-grant delivery can be
done via

-   OAuth2 code-grant flow

-   Back-channel i.e. Direct injection to application (e.g. Via secrets in Kubernetes)

![](./media/Application-Bootstrap.png)

All tokens in TrustFabric specification are short duration tokens, which
requires a mechanism to re-evaluate trust and replenish the referral
tokens to the application.

Once application receives the referral token (a.k.a. Code grant), it can
use the token flow to obtain access tokens with identity and
authorization claims.

![](./media/Application-token.png)

Token Formats
-------------
### Signing
### Encryption

Token Verification
------------------

Access Validation
-----------------

OIDC Tokens
===========

UMA Tokens
==========

OAuth2 Tokens
=============
