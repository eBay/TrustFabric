# TrustFabric Identity specification

<i>**Note**: Before you begin, in this specification terms <u>Application</u> and <u>Service</u> are used interchangeably, specification applies to <u>both</u>. This specification follows the definition as proposed by Jacob Jenkov [here](http://tutorials.jenkov.com/soa/services-applications.html)</i>

TrustFabric is an extensible Cloud Native Identity Specification for Applications. Specification includes:

* Application Identity Representation
* Application Identity Association
* Application Identity Verification (Authentication)
* Application Authorizations and Verification mechanisms
* Identity revocation and invalidation
* Interoperability
* Extensibility and Canonical forms

## Why we needed yet another specification
Security landscape is changing fast. Adoption of micro-services and cloud-native technology has changed the threat landscape. Here are few challenges
1. Services interact with each other and they need an identity
1. Service impersonation is a new attack vector
1. Service security is still dependent on static credentials 
1. **Confused deputy** is new attack dimension with micro-services
1. Holistic approach of IDM/IAM does not exist for applications
1. Diversity of applications make it harder to standardize

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



