# TrustFabric Overview

Introduction
============

TrustFabric is an open-source specification and implementation for
Cloud Native Identity and Access management. It secures interactions
between applications and between users and applications.

TrustFabric is an extension of OAuth2, OIDC and UMA 2.0 specifications. 
It establishes a strong application identity representation for handing 
the challenges associated with microservices, e.g. compromised application
identity and credentials, larger attack surface and confused deputy.

Challenges
==========

Enterprises typically have a multitude of heterogeneous applications
(a.k.a. services). These applications have different architecture, they
speak different protocols, they are built using different programming
languages and they have different authentication and access
requirements. Enterprises may also have specialized deployment topology,
multiple lifecycles and different security zones (e.g. SOX, PCI) for
hosting applications.

Summarizing these challenges:

-   Multiple Authentication mechanisms

-   Lack of standardization of Application Identity and Verification

-   Lack of unified policies and rules for access and authorizations

-   Scale and time-sensitivity

-   Weak security model

To solve these challenges, a comprehensive, secure, interoperable and
flexible approach is needed.

Goals
=====

1.  Define models to align application lifecycle for strong **password-less** 
    identity and access. Essentially any static credential that can be used
    to impersonate an application should be avoided.

1.  Standardize identity representation, bootstrapping, verification, and 
    validation

1. Improve resiliency and reliability by making authentication, access and authorization decision local

1.  Federate application identity across clusters

1.  Standardize authentication and authorization for User-to-App, App-to-App, 
    User-to-App-to-App and App-to-App-to-App scenarios

1.  Improve security posture by eliminating static secrets, reducing attack surface and bringing accountability.

1.  Achieve interoperability (OAuth2, OIDC, UMA 2.0 etc) and ease of adoption

1.  Provide a reference implementation

Overview
========

Securing application interactions using TrustFabric 
---------------------------------------------------

TrustFabric leverages JWT based Application Identity and Access Tokens for representing client application identity. Token represents the complete identity of the client application and can be validated locally by target service without external
interaction/dependencies. Since tokens are short duration tokens,
renewal will ensure updates on claims are available in the token. All interactions happen over TLS 1.2+ enabled communication. Since interactions may be initiated by a users, job or an agent, TrustFabric build interoperability with OIDC/OAuth2 and extends it for application identities and interactions. The on-behalf-of and authorized presenter functionality helps in retaining the initiator context during app-to-app interactions. Mutual authentication is achieved by leveraging TLS 1.2 verification based on RFC 6125 for the server and JWT based client identity. Access policies are embedded as claims for the client application, this makes authentication and authorization completely local and eliminates dependencies on external services.

![](./media/Application-overview.png)


### Fabric of trusted applications
TrustFabric relies on building a strong fabric for interactions between trusted application. The trusted fabric is built using 
1.  Encrypted communication with strong server network identity. This is achieved by using TLS 1.2+ with server certificates and RFC 6125 compliant verification
1.  Dynamic, trust aware, transparent, non-repudiation  and MitM safe client tokens. This is covered in details in the [Identities](./Identity.md) section
1.  Secure handshake that establishes the authenticity of communicating parties and a secure channel that is allowed by access policy embedded as token scope. This is covered in details in [TrustFabric](./TrustFabric.md) section

This allows secure and trusted communication that is allowed based on policies and best-practices for securing interactions in both private and public network scenarios.   
*Note: Additional controls like application firewall, network ACL etc might have to be enabled based on security requirements, these controls will be covered by the specification in the future.*

### Securing Application flows End-to-End
Application interactions are more complex than simple app-to-app interactions. With micro-services there is increased attack surface and more complex interaction patterns e.g. user-to-app-to-app and app-to-app-to-app where source identity context might have to be propagated across multiple applications. E.g. a user context might have to be passed with verification capabilities across multiple micro-services to avoid confused deputy vulnerability.
Specification achieves this by building interoperability and profiles for OIDC and UMA 2 specifications. Details are covered in [Identities](./Identity.md) and [TrustFabric](./TrustFabric.md) sections.


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



TrustFabric Control Plane Components
------------------------------------

### Federated Control Plane Components

-   Federated Identity Stores (Application, Users, Groups, Zones)

-   Federated Policy Store (Admission, User-App, App-App,
    User-Resource,..)

-   Authentication and token services (OIDC, OAuth2, LDAP, KDC)

-   Key and Certificate Services/Agents

### Distributed Control Plane Components

-   Bootstrap Controllers (CodeGrant)

-   Admission Webhooks

-   Application Specific Sidecars (X.509, KRB keytab and
    Identity/AccessToken delivery)

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
