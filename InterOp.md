# Interactions and Interoperability
TrustFabric separates the interactions in two layer. First layer is *trusted underlay*, which is a fabric of trusted applications, while the second layer is *trusted overlay* which represents the session aspect of interactions initiated by users and apps via delegation of authorization.

Building Fabric of trusted applications (Underlay)
==================================================
Establishing a trusted fabric of applications (i.e. Underlay) is done in two steps, 
1. Establishing application identity and 
2. Establishing access patterns

The application identity is established by injecting code-grant via bootstrapping mechanism (bootstrap/referral token). Application can now use the code-grant to obtain the identity and access token using `token` endpoint of the authorization server.

Application Referral Token Injection
------------------------------------

### OAuth2 Authorization Code Flow
#### Authorization end-point

### Back Channel Controller

#### Back channel implementation for Kubernetes

1. Identity Controller
1. Admission Controller

Application Identity and Access Tokens
--------------------------------------
## Token Endpoint





Referral Token
--------------


### Authorization Endpoint


Access Token
------------
### Token endpoint
#### Interaction

### Envoy Integration

Interoperability
=================

GRPC Based Integrations
-----------------------

SASL Based Integrations
-----------------------

OAuth2 and OIDC Interoperability
================================

