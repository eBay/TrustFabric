# TrustFabric Identity specification

## Applications vs Services

**Note**: *This specification follows the definition as proposed by Jacob Jenkov [here](http://tutorials.jenkov.com/soa/services-applications.html)*

* Applications are accessed by users and may be accessed by other programs over network
* Services are accessed by programs and may be accessed by users over network
* In cloud-native world, both applications and services perform specialized operations.

In this specification terms <u>Application</u> and <u>Service</u> are used interchangeably, specification applies to <u>both</u>.

## What is TrustFabric

TrustFabric is an extensible Cloud Native Identity Specification for Applications. Specification includes:

* Application (a.k.a. Service) Identity Representation and Identity injection
* Application Identity Verification (Authentication) and Authorizations
* Identity revocation and invalidation
* Interoperability and Extensibility

## Why we needed yet another specification

Security landscape is changing fast. Adoption of micro-services and cloud-native technology has changed the threat landscape. Here are few challenges:

1. Applications (a.k.a. Services) interact with each other and they need an identity
1. Application impersonation is a new attack vector
1. Application security is still dependent on static credentials 
1. **Confused deputy** is new attack dimension with micro-services
1. Holistic approach of IDM/IAM does not exist for applications
1. Diversity of applications make it harder to standardize

## Navigating the Documentation

Following sections provide a details for the specification:

* [Terminology](./Terminology.md)
* [Goals](/Goals.md)
* [Overview](./Overview.md)
* [Fabric of Trusted Applications](./TrustFabric.md)
* [Architecture](./Architecture.md)
* [Identity Representation](./Identity.md)
* [Tokens and Validations](./Tokens.md)
* [Interactions and Protocol](./InterOp.md)
* [LDAP Integration](./LdapSchema.md)
* Service Mesh Integration
* FAQ



