# User Identities
TrustFabric specification for user identities applies to your OpenID Connect implementation (OIDC server).

## BYO IdM

TrustFabric allows you to leverage your existing investments in IdM solutions. You can continue to leverage existing implementation. TrustFabric provides specifications for OpenID Connect server for integration:
1. Implement application identities (`client-id`) in a password-less manner using Fabric layer
2. Implement `azf` claims if you want to support multi-party delegation (transitive delegation)
3. Extend the scope and claims based on TrustFabric specifications for claims based authorization

## User Representation

TrustFabric representation leverages OAuth2, OIDC and UMA2 specifications for standardization of identity claims and access delegations. User identity representation implementation is outside the scope of this specification. Objective of the specification is to provide interoperability via adoption of standards (i.e. OIDC, UMA2 and OAuth2).
User subject claim must use a unique identifier. It is recommended to follow LDAP Distinguished name syntax for user identities, though it is not mandatory. Alternative unique identifiers e.g. Email can also be used as subject claim.

### Integration with TrustFabric
1. IAM and IdM integration with TrustFabric happens at Session layer
1. Implement TrustFabric application identities for OAuth2/OIDC client application registration
1. Leverage `azp` claim to indicate client identity (TrustFabric DN for the client) 
1. Optionally leverage `azf` claims for multi-level delegation. Please note `azf` claim is a TrustFabric specific claim
1. TrustFabric allows you to define custom scope and claims for implementing claim based authorization.

[TODO] Move following sections to Authorizations

## User Tokens
Although specification does not provide mechanism to authenticate users, it provides integration with OIDC specifications and it leverages JWT based identity and Access Tokens.  
***Note**: Please check the applicable changes to the **/token** endpoint request for the token request flow for the **Authorization** header and value of the client credentials.*

### TrustFabric specific claims
* Authorized Forwarder (`azf`): Claim is useful in representing `resource-servers` which may act like a `relying-party`. This typically happens when a `resource-server` needs to call another `resource-server` in context of the user. The OIDC `azp` claim is not sufficient as it only represents the `client` or the `relying-party`. Claim if present should be in the access token and not in the identity token.


## Authorization Scope and Claims
TODO
