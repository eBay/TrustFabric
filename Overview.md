# TrustFabric : Cloud-Native Application Centric Identity and Access Management

Introduction
============

TrustFabric is an open-source specification and implementation for an application
centric Cloud-Native Identity and Access management. It secures interactions
between applications and between users and applications.

TrustFabric is an extension of OAuth2, OIDC and UMA 2.0 specifications. 
It establishes a strong application identity representation for handling 
the challenges associated with microservices, e.g. compromised application
identity and credentials, larger attack surface and [confused deputy](https://en.wikipedia.org/wiki/Confused_deputy_problem).

Challenges
==========

Enterprises typically have a multitude of heterogeneous applications
(a.k.a. services). These applications have different architecture, they
speak different protocols, they are built using different programming
languages and they have different authentication and access
requirements. Enterprises may also have specialized deployment topology,
multiple life-cycles and different security zones (e.g. SOX, PCI) for
hosting applications.

Summarizing these challenges:

-   Multiple Authentication mechanisms

-   Lack of standardization for application identity and verification

-   Lack of unified policies and rules for access and authorizations

-   Scale and time-sensitivity

-   Weak security model

To solve these challenges, a comprehensive, secure, interoperable and
flexible approach is needed.

Goals
=====

1.  Standardize identity representation, bootstrapping, verification, and 
    validation

1.  Federate application identities across clusters, security-zones and life-cycles

1.  Improve security posture by eliminating static secrets (**password-less**), reducing attack surface and bringing accountability. Trust and dynamic evaluation of trust are the new security anchors for application identity. 

1.  Improve resiliency and reliability by making authentication, access and authorization decision local

1.  Standardize authentication and authorization for User-to-App, App-to-App, 
    User-to-App-to-App and App-to-App-to-App scenarios

1.  Achieve interoperability (OAuth2, OIDC, UMA 2.0 etc) and ease of adoption

1.  Provide a reference implementation

Overview
========
TrustFabric implements security using two layer implementation:
1. Fabric - Underlay that provides secure application interaction
2. Session - Optional overlay that provides session information about the request initiator for transaction processing


TrustFabric leverages JWT based Application Identity and Access Tokens for representing client application identity. Token represents the complete identity of the client application and can be validated locally by target service without external
interaction/dependencies. Since tokens are short duration tokens,
renewal will ensure updates on claims are available in the token. All interactions happen over TLS 1.2+ enabled communication.  
Mutual authentication is achieved by leveraging TLS 1.2 verification based on RFC 6125 for the server and JWT based client identity. Access policies are embedded as claims for the client application, this makes authentication and authorization completely local and eliminates dependencies on external services.

![](./media/Application-overview.png)

Since interactions may be initiated by a users, job or an agent, TrustFabric build interoperability with OIDC/OAuth2 and allows delegation of authorization to application identities and thus allowing the interactions. The on-behalf-of and authorized presenter functionality helps in retaining the initiator context during app-to-app interactions.
![](./media/TrustFabric-UserSession.png)

### Fabric of trusted applications
TrustFabric relies on building a strong fabric for interactions between trusted application. The trusted fabric is built using 
1.  Encrypted communication with strong server network identity. This is achieved by using TLS 1.2+ with server certificates and RFC 6125 compliant verification. This provides confidentiality and server authentication
1.  Dynamic, trust aware, transparent, non-repudiable  and MitM safe client application tokens. This is covered in details in the [Identities](./Identity.md) section. This provides the mutual authentication by providing verifiable client application identity to server application
1.  Policy based access which is locally verifiable. This is covered in details in [TrustFabric](./TrustFabric.md) and [Interoperability](./InterOp.md) sections.

This allows secure and trusted communication that is allowed based on policies and best-practices for securing interactions in both private and public network scenarios.   
*Note: Additional controls like application firewall, network ACL etc might have to be enabled based on security requirements, these controls will be covered by the specification in the future.*

### Managing Sessions End-to-End
Application interactions are more complex than simple app-to-app interactions. With micro-services there is increased attack surface and more complex interaction patterns e.g. user-to-app[-to-app]<sup>\+</sup> and app-to-app[-to-app]<sup>\+</sup> where source identity context might have to be propagated across multiple applications. E.g. a user context might have to be passed with verification capabilities across multiple micro-services to avoid confused deputy vulnerability.
Specification achieves this by building interoperability and extending profiles for OIDC and OAuth2 specifications. Details are covered in [Tokens](./Tokens.md) and [TrustFabric](./TrustFabric.md) sections.


Representing application identity
---------------------------------

Typically, business functions consist of multiple components e.g.
Frontend Web/Mobile, Application APIs, databases, agents, jobs etc. 
These components are maintained by different teams and typically are a 
combination of multiple technologies (micro-services, stateful workloads 
etc). Each of these components (a.k.a. applications) can have multiple 
deployments (a.k.a. AppInstances) in different zones.

![](./media/Application-Components.png)

Application identity is represented as a JWT tokens, Kerberos tokens or X.509 certificates, where identity subject is represented using LDAP/X.509 Distinguished naming syntax. Application primary method of identification is a JWT token, while Kerberos and X.509 certificates are used to represent derived and network service identities.

Ecosystem: OIDC, OAuth2, UMA and TrustFabric
---------------------------------------
* OAuth2 is the center of the universe
* OAuth2 is a delegation mechanism typically used by user to delegate access authorizations to a client application. This allows client application to access user's resources.
* UMA2 is a authorization mechanism built for user to grant other users an access to resource they own in a controlled manner
* OIDC provides mechanism to authenticate users and identify user from client application perspective
* TrustFabric serves aspect of OAuth2 and OIDC in app-app context
  * TrustFabric provides an identity and authentication for client application. The `client_id` field and `client_secret` are `sub` claim and the access token provided by TrustFabric to the client
  * TrustFabric provides mechanism to authorize application to application access
  * TrustFabric allows OIDC/OAuth2 based delegations in user-app-app or app-app-app interaction pattern  

Diagram below compares OAuth2, OIDC, UMA2 and TrustFabric  
![](./media/Application-Compare.png)

  
TrustFabric Control Plane Components
------------------------------------

### Federated Control Plane Components

-   Federated Identity Stores (Application, Users, Groups, Zones)

-   Federated Policy Store (Admission, User-App, App-App,
    User-Resource,..)

-   Authentication and token services (OIDC, OAuth2, LDAP, KDC)

-   Key and Certificate Services/Agents

### Distributed Control Plane Components

-   Bootstrap Controllers (CodeGrant, X.509 Certs, Keytabs/KRB tokens)

-   Admission Webhooks

-   Application Specific Sidecars 
    -   Identity, access token refresh and delivery
    -   Verification of Caller Identity and Claims

Standards and Specifications
============================

1.  OAuth2.0 [[RFC 6749]](https://tools.ietf.org/html/rfc6749)

1.  Open ID Coneect Specifications: [[OIDC]](https://openid.net/developers/specs/)

1.  JWT profile for OAuth2.0 [[RFC 7523]](https://tools.ietf.org/html/rfc7523)

1.  ITU [[X.509]](https://www.itu.int/rec/T-REC-X.509/en)

1.  SASL for OAuth [[RFC 7628]](https://tools.ietf.org/html/rfc7628)

1.  X.500/LDAP DN representation [[RFC 4514]](https://tools.ietf.org/html/rfc4514)

1.  Representation and Verification of Domain-Based Application Service
    Identity within Internet Public Key Infrastructure Using X.509 (PKIX)
    Certificates in the Context of Transport Layer Security (TLS)[[RFC 6125]](https://tools.ietf.org/html/rfc6125)
