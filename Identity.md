# Identities

* [Identity Naming](./identity/Naming.md)
* [User Identity](./identity/Users.md)
* [Application Identity](./identity/Applications.md)
    * LDAP Representation
    * Kubernetes Model
* Mapping Identities in your environment


## Naming Format
TrustFabric specification using X.500 naming model (a.k.a. LDAP Distinguished Naming model) for representing identities. Specification does not provide any model for user identities, though it does provide specifications for application naming. Although, LDAP/Directory syntax is used for naming, there is no requirement to have LDAP directory for storing identities. Usage of X.500 naming allows interoperability with LDAP, X.509 certificates etc.

User Identities
=============
Please refer [User identity](./identity/Users.md) section.




Quick Note of LDAP DNs:

1. LDAP `DN` (Distinguished name) is formed by adding all `RDN` values (Relative distinguished name) from child (left) to parent(right) separated by `,` (comma). Spaces before and after `,` are optional and ignored unless escaped (Please refer LDAP RFC 4514)
1. Every LDAP entry has a unique RDN under a parent
1. Each RDN consists of multiple RDN Components joined using `+` (plus) sign. Most RDN values will have just one component. Spaces before and after `+` are optional and ignored unless escaped (Please refer LDAP RFC 4514)
1. Each RDN component is a `key` and `value` pair separated by `=` (equal) sign. Spaces before and after `=` are optional and ignored unless escaped (Please refer LDAP RFC 4514)
1. Do not use a string parser or string match to compare `DN` or `RDN` values. Instead implement a DN comparator provided by LDAP SDK being used.
1. In LDAP each RDN value corresponds to a object with components being attribute/value pair of that object. Object may have more attribute/values pairs and the object is created as composition of multiple object-classes (just like OOP).
1. TrustFabric only uses LDAP DN syntax only.  *Use of LDAP is completely optional and in many cases not required*.

Mapping Identities
==================
TrustFabric allows identity creation via mappings. This allows easier and flexible adoption of identity representation in your environment.

Naming
------
LDAP DN (distinguished naming) syntax is used for representing the application identities. Objects used for representing application identity can be modelled in many ways. This specification covers two mechanisms as reference.
1. Mapping based on LDAP
1. Mapping based on Kubernetes CRDs

Mapping Identities in LDAP
---------------------------
LDAP mapping is needed if your organization uses LDAP directories or IdM/IAM systems backed by LDAP directories. LDAP based identity representation makes it easier to achieve federation for application identities and can also be leveraged to build application catalog.
Please refer to [LDAP Schema section](./LdapSchema.md) for LDAP based identity modelling.

Mapping Identities in Kubernetes
--------------------------------
In Kubernetes following objects are available for mapping applications:
1. Clusters
1. Namespaces
1. Deployments

**Important**: TrustFabric is a federated implementation for application and user identities. Based on your implementation, additional controls may be required to implement unique identities across a multi-cluster Kubernetes infrastructure.
