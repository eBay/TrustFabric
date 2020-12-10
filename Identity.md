# Identities

* [Identity Naming](#identity-naming)
* [User Identity](./identity/Users.md)
* [Application Identity](./identity/Applications.md)
  * [LDAP Representation](identity/LDAP.md)
  * [Kubernetes Model](identity/K8S.md)
* [Mapping Identities in your environment](identity/Mapping.md)

## Identity Naming

TrustFabric specification uses X.500 naming model (a.k.a. LDAP Distinguished Naming model) for representing identities. Although, LDAP/Directory syntax is used for naming, there is no requirement to have LDAP directory for storing identities. Usage of X.500 naming allows interoperability with LDAP, X.509 certificates etc.

Following formats are supported:

1. **LDAP Distinguished Name** - Distinguished name string (`DN`) syntax provides a unique locator based model for representing identities. The `DN` representation must follow the [RFC 4514](https://tools.ietf.org/html/rfc4514). This is a default option in the TrustFabric specification.
1. **LDAP URL Syntax** - In some cases (e.g. Policies, Certificate SAN entries), LDAP URL format is supported as well. LDAP URL format allow representing one or more identities (as URL format allows representing search query). LDAP URL syntax must follow [RFC 4516](https://tools.ietf.org/html/rfc4516)

Although both `DN` and `LDAP URL` syntax are supported, specification assumes `DN` syntax as default. 

Quick Note of LDAP DNs:

1. LDAP `DN` (Distinguished name) is formed by adding all `RDN` values (Relative distinguished name) from child (left) to parent(right) separated by `,` (comma). Spaces before and after `,` are optional and ignored unless escaped (Please refer LDAP RFC 4514)
1. Every LDAP entry has a unique RDN under a parent
1. Each RDN consists of multiple RDN Components joined using `+` (plus) sign. Most RDN values will have just one component. Spaces before and after `+` are optional and ignored unless escaped (Please refer LDAP RFC 4514)
1. Each RDN component is a `key` and `value` pair separated by `=` (equal) sign. Spaces before and after `=` are optional and ignored unless escaped (Please refer LDAP RFC 4514)
1. Do not use a string parser or string match to compare `DN` or `RDN` values. Instead implement a DN comparator provided by LDAP SDK being used.
1. In LDAP each RDN value corresponds to a object with components being attribute/value pair of that object. Object may have more attribute/values pairs and the object is created as composition of multiple object-classes (just like OOP).

*Use of LDAP Directory is completely optional and in many cases not required*.
