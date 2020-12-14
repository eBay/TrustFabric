# TrustFabric Identity specification

## Applications vs Services

**Note**: *This specification follows the definition as proposed by Jacob Jenkov [here](http://tutorials.jenkov.com/soa/services-applications.html)*

* Applications are accessed by users and may be accessed by other programs over network
* Services are accessed by programs and may be accessed by users over network
* In cloud-native world, both applications and services perform specialized operations

**Note**: Although the term application and service is used interchangeably in this document, TrustFabric uses *Application* as a standard term. The specification applies to both.
 
## What is TrustFabric

TrustFabric is an extensible Cloud Native Identity Specification for Applications. Specification includes:

* Application (a.k.a. Service) Identity Representation and Identity Injection
* Application Identity Verification (Authentication) and Authorization
* Identity Revocation and Invalidation
* Interoperability and Extensibility

## Why yet another specification?

Security landscape is changing fast. Adoption of micro-services and cloud-native technology has changed the threat landscape. Here are few challenges:

1. Applications (a.k.a. Services) require an identity when interacting with each other
1. Application impersonation is a new attack vector
1. Application security is still dependent on static credentials 
1. **Confused deputy** is new attack dimension with micro-services
1. Holistic approach of IDM/IAM does not exist for applications
1. Diversity of applications make it harder to standardize

## Navigating the Documentation

Following sections provide a details for the specification:

* [Terminology](./Terminology.md)
* [Goals](./Goals.md)
* [Overview](./Overview.md)
* [Fabric of Trusted Applications](./TrustFabric.md)
* [Interactions and Protocol](./InterOp.md)
* [Architecture](./Architecture.md)
* [Identity Representation](./Identity.md)
* [Tokens and Validations](./Tokens.md)
* [Integration](./Integrations.md) - gRPC, Envoy, Service Mesh, Generics and more
* [Identity Protection](./StrongIdentity.md) - MitM, Confused deputy, replay attacks
* FAQ



