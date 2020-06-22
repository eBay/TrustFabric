TrustFabric Tokens
==================
Application Tokens
------------------
TrustFabric implements three tokens for applications:
1. Bootstrap token (a.k.a. Referral tokens)
1. Access token
1. Refresh tokens

While tokens establish the client identity, specification supports X.509 certificates for TLS termination and network identity of server. Specification also supports derived authentication credentials e.g. Kerberos tokens for client authentication. Derived authentication credentials are obtained by using the  access tokens.

### Bootstrap token
Bootstrap tokens (a.k.a. Referral tokens) are equivalent to OAuth2 code-grant. These are referral tokens issued to application by a trusted referrer. These are implemented as JWT tokens and must contain aud (application) and sub (referrer), besides standard JWT claims. In example below user John Doe (jdoe) from acme.org has issued a code-grant bootstrap token to a cart application pool deployed in production environment:
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
  "sub": "uid=jdoe, ou=platform, o=people, dc=users, dc=acme, dc=org",
  "client_id": "cn=cartapp-1 + L=production, ou=cartapp, o=cart, dc=apps, dc=acme, dc=org",
  "aud": "cn=authsrv-1 + L=production, ou=authsrv, o=trustfabric, dc=apps, dc=acme, dc=org",
  "inst": [
    "10.147.185.209"
  ],
  "vot": "P1.Cc.Ac",
  "vtm": "https://trustmark.acme.org/trustmark/v2",
  ...
}
```
In the token above 
- `iss` represents the issuer, 
- `sub` represents the referrer identity subject (LDAP DN used here), 
- `client_id` represents the client application identity, 
- `aud` represent the audience for the referral token i.e. authorization server (identity is represented using LDAP DN). 
- `inst` is the instance claim and is an array of IP addresses to which this token is issued. These addresses should represent the network identity of the application (`client_id`). Claim can be used to perform the trust verification via reverse lookup for the `token` flow and can also be used to do origin validation during token verification.
- `vot` and `vom` are the optional vector of trust claims.

### Access token
Access tokens establish the client identity and also represent the access claims
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
  "sub": "cn=cartapp-1 + L=production, ou=cartapp, o=cart, dc=apps, dc=acme, dc=org",
  "aud": [
    "cn=cartdb-1 + L=production, ou=cartdb, o=cart, dc=apps, dc=acme, dc=org",
    "cn=configapp-1 + L=production, ou=configapp, o=cart, dc=apps, dc=acme, dc=org",
    "cn=vaultapp-1 + L=production, ou=vaultapp, o=vault, dc=apps, dc=acme, dc=org"
  ],
  "inst": [
    "10.147.185.209"
  ],
  "vot": "P1.Cc.Ac",
  "vtm": "https://trustmark.acme.org/trustmark/v2",
  ...
}
```
In the token above 
- `iss` represents the issuer i.e. Trustfabric authorization server, 
- `sub` represents the application identity as LDAP DN, 
- `aud` represent the audience for the access token. Essentially representing the **access claims of the application**,
- `inst` is the instance claim and is an array of IP addresses to which this token is issued. These addresses should represent the network identity of the application (`sub`). Claim can be used to perform the trust verification by audience of the token,
- `vot` and `vom` are the optional vector of trust claims.

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
  "sub": "cn=cartapp-1 + L=production, ou=cartapp, o=cart, dc=apps, dc=acme, dc=org",
  "aud": "cn=authsrv-1 + L=production, ou=authsrv, o=trustfabric, dc=apps, dc=acme, dc=org",
  "inst": [
    "10.147.185.209"
  ],
  "vot": "P1.Cc.Ac",
  "vtm": "https://trustmark.acme.org/trustmark/v2",
  ...
}
```
In the token above 
- `iss` represents the issuer i.e. Trustfabric authorization server, 
- `sub` represents the application identity as LDAP DN, 
- `aud` represent the audience for the referral token i.e. authorization server (identity is represented using LDAP DN). 
- `inst` is the instance claim and is an array of IP addresses to which this token is issued. These addresses should represent the network identity of the application (`sub`). Claim can be used to perform the trust verification by audience of the token,
- `vot` and `vom` are the optional vector of trust claims.


Obtaining Tokens
-----------------
Applications share identity with other applications on the network for
app-app interactions. Application needs to be bootstrapped with identity
for these interactions. TrustFabric is a password-less mechanism and
implements a two step mechanism to establish identity.

-   Referral identity injection using bootstrap token (by trusted referrer)

-   Obtaining Identity and Access tokens for interactions using referral identity (bootstrap tokens)

Application needs to be bootstrapped with referral identity token (A.K.A code grant)

Bootstrapping Application Identity
----------------------------------
TrustFabric is a password-less implementation, so applications do not have a static credential. 
Instead, applications obtain the access token via a referral token, issued by a trusted entity 
e.g. an authenticated admin user or by a trusted controller which has already established a 
TrustFabric based identity. Referral tokens are implemented using OAuth2 code grant and JWT 
representation. Specification extends the delivery model of Code grant, where code grant 
delegation can happen via:
1. Dev Ops user authorizing application using OAuth2 Authorization Code Grant Flow based 
delivery of code grant
1. Infrastructure controller using a back-channel mechanism to deliver code grant obtained, where 
infrastructure controller may have been authorized by Dev ops user defined in previous step

As part of referral identity injection, application deployment is provided with
a short duration referral identity token by TrustFabric, based on the trust declared
by a trusted referrer e.g. DevOps managing applications or a Kubernetes operator. The
referral token implements OAuth2 semantics of code-grant, where referrer token identity
is tied to referrer identity (using `sub` claim), while application identity is captured
using `aud` claim. The `aud` claim may contain multiple application identities ( this is 
non recommended as it weakens the security posture). The `aud` claim may contain the authorization
server identity as well , if the implementation of authorization server requires it.
This referral identity can later be used by application to obtain identity and access 
tokens. TrustFabric leverages OAuth2 to implement token request flows. *It is important 
to note that the token interaction produces access tokens with application's identity and 
not the tokens for delegated access*. Identity, subject and access claims embedded in the 
token are for the application. Unlike OIDC, there are no separate identity tokens. The 
access token is implemented as transparent token using JWT and can be used as application 
identity token (has identity claims embedded). The 
referral tokens are treated as code-grant issued by DevOps or by Operators. 
Code-grant delivery can be done via

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

Please refer to **[Token Interactions and Interoperability section](./InterOp.md)** for more details.

Token Identifier
----------------
Although JWT is a transparent token implementation, there are scenarios where a unique identifier for a token is needed. This includes Security logging, Token revocations etc. Specification provides following guidelines for creation of unique token identifier:
*   Use `jti` claim to represent the unique identifier
*   Unique identifier should include checksum of claims representing at least `sub`, `aud`, access and authorization claims
*   Avoid using `iat`, `nbf` and `exp` claims in the checksum
*   Use small clear-text identifier portion of the subject for cross reference
*   A time-bucket based implementation (e.g. TOTP) can be used to add time factor while generation of `jti` claim. Caution is needed to account for a clock skew, e.g. jitter can be defined to account for acceptable clock skew
*  The checksum, clear text and time based components can be combined to create unique identifier. This identifier will stay same for the given time-bucket as long as no claims are changed

Token Formats
-------------
## Underlay Supported Schemes
Following recommendations are for *application identity and access tokens*, representing the underlay network:
### Recommended Digital Signature Algorithms
Following JWT signing schemes are recommended based on performance and security requirements of short duration tokens for applications:
1. RSA - Scheme with RSA asymmetric key pair of size 2048 or higher with SHA256 or SHA384. Sub-variants include:
  * RS256 - RSA PKCS#1 signature using SHA-256 hash algorithm 
  * RS384 - RSA PKCS#1 signature using SHA-384 hash algorithm 
  * PS256 -  RSA PSS signature using SHA-256 hash algorithm
  * PS384 -  RSA PSS signature using SHA-384 hash algorithm
1. ECDSA - Scheme with Elliptic curve digital signing algorithm. Sub-variants include:
  * ES256 - ECDSA using P-256 curve and SHA-256 hash algorithm
  * ES384 - ECDSA using P-384 curve and SHA-384 hash algorithm
1. EdDSA (OKP) - Scheme with Edwards-curve DSA signature and SHA-2 hashing function. Sub-variants include:
  * Ed25519

**Note**: The digital signature algorithms provide *Integrity, Authenticity and non-repudiation* characteristics to the tokens. They do not provide confidentiality, which makes them unsuitable for sessions and for user tokens requiring confidentiality.


### Prohibited
1. None : The `"alg":"none"` usage is prohibited in TrustFabric implementation due to security vulnerabilities and exploits
1. HMAC with SHA-2: HS256, HS384 and HS512 algorithms are not allowed for application tokens

## Nested signing and encryption
For session objects and where confidentiality is needed, nested signing and encryption is recommended. This requires leveraging JWE to encrypt a JWT token. 

Token Verification
------------------
Application tokens verification requires:
1. Key download and verification: TrustFabric default implementation for JWT requires `alg`, `kid` and `x5u` header claims,
1. The issuer (`iss`) verification with x5u pointed subject subject and SAN entries matching trusted issuer configuration,
1. JWT signature verification using `x5u` and `kid` claims,
1. Validity check against `exp`, `nbf` and `iat` claims
1. If `inst` claim is present, it should be verified against the client IP (or Client IP from X-Real-IP/X-Forwarded-For headers).


Access Validation
-----------------
Target system can verify the access claims by:
1. Token verification
1. Reading `aud` claim and checking for target system application DN in the audience list.
  - Target application may implement additional policy to allow access based on fine-grained policies maintained separately
  - Target application may allow access to application based on origin application's identity attributes, e.g. if origin application is from production zone vs QA zone, difference policy may be applicable for the requested resources.

Roadmap for Access Control
--------------------------
1. Integrate with OPA (Open Policy Agent)
1. Integrate with Istio for authorization policies

Token Revocation List
----------------------
