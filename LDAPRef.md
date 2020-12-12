# LDAP Reference for Object Model

## LDAP OID Organization

TrustFabric uses Private Entity Number (PEN) provided by [IANA](https://www.iana.org) for defining LDAP attributes, objectclasses and other LDAP objects. To maintain extensibility TrustFabric adopts following model for maintaining internal OIDs. Scheme presented here is inspired and aligned with Microsoft and [OpenLDAP](https://www.openldap.org/faq/data/cache/197.html)

Note: IANA OID structure following following scheme and this should not be changed.
IANA OID Strucure
[iso].[org].[dod].[internet].[private].[enterprise].[PEN].[Entity Specific Structure]
Where,  

1. iso - 1
1. org - 3
1. dod - 6
1. internet - 1
1. private - 4
1. enterprise - 1
1. **PEN TrustFabric - 55988**

Object-classes, attributes and other object will share a OID prefix `1.3.6.1.4.1.55988` in TrustFabric specifications.

### Entity Specific Structure

Usually enterprises are free to choose structure appropriate for their usage. For standardization, there are specific recommendations e.g. [OpenLDAP](https://www.openldap.org/faq/data/cache/197.html) here provides a good reference.

Typically following format is used for LDAP objects

- 1.3.6.1.4.1.[PEN].1 - assign to SNMP objects
- 1.3.6.1.4.1.[PEN].2 - assign to LDAP objects
- 1.3.6.1.4.1.[PEN].2.[N] - For various LDAP Objects

Where N can be integer representing LDAP attributes, classes, syntax, matching rules etc.

### TrustFabric specific structure

Considerations

- Internal Usage : TrustFabric specific core object models
- External Contributions to TrustFabric: Open-source contributions need identification and ownership within TrustFabric scope
- External Proprietary : Out of scope, as they are not relevant for TrustFabric common specifications. Should not be included under TrustFabric OID.

To allow for extensibility following structure is adopted:

1.3.6.1.4.1.[TrustFabric PEN].[Org Unit].[PROTOCOL].[N].[ID]

Where:

- IANA Prefix =1.3.6.1.4.1
- TrustFabric PEN = 55988
- Org Unit = [1-50: TrustFabric Internal, 51-100: Reserved, 101+ External Contributions ]
- Protocol = [1: SNMP, 2: LDAP etc]
- N = Object types defined based on [OpenLDAP](https://www.openldap.org/faq/data/cache/199.html) recommendations
  - Syntaxes:1
  - Matching Rules: 2
  - Attribute Types : 3
  - Object Classes : 4
  - Supported Features : 5
  - Protocol Mechanisms : 9
  - Controls : 10
  - Extended Operations : 11
- ID = Object ID

## Example
OID : `1.3.6.1.4.1.55988.1.2.4.101`  
In this example:

* `1.3.6.1.4.1.55988` is the TrustFabric registered OID prefix
* <u>1</u> represents Org ID = 1 (for TrustFabric internal use)
* `2` represents the **LDAP protocol**
* **4** represents object-class
* `101` is the ID assigned in object class under prefix `1.3.6.1.4.1.55988.1.2.4`


## ORG ID assignments
Following block represents the ORG ID assignments. Please raise a PR to reserve the ORG ID for TrustFabric schema submissions:
| Company | EMAIL | ID ASSIGNED |
| --- | ----- |---- |
| ACME INC | ldap-schema-dl@acme.org | 101 |





