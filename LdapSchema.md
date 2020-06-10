#LDAP Schema and Organization

## LDAP OID Organization
eBay uses Private Entity Number (PEN) provided by [IANA](https://www.iana.org) for defining LDAP attributes, objectclasses and other LDAP objects. To maintain extensibility eBay adopts following model for maintaining internal OIDs. Scheme presented here is inspired and aligned with Microsoft and [OpenLDAP](https://www.openldap.org/faq/data/cache/197.html)

Note: IANA OID structure following following scheme and this should not be changed.
IANA OID Strucure
[iso].[org].[dod].[internet].[private].[enterprise].[PEN].[Entity Specific Strucutre]
Where,
iso - 1
org - 3
dod - 6
internet - 1
private - 4
enterprise - 1
**PEN eBay - 55805**

### Entity Specific Structure
Usually enterprises are free to choose structure appropriate for their usage. For standardization, there are specific recommendations e.g. [OpenLDAP](https://www.openldap.org/faq/data/cache/197.html) here provides a good reference.
Typically following format is used for LDAP objects
1.3.6.1.4.1.[PEN].1 - assign to SNMP objects
1.3.6.1.4.1.[PEN].2 - assign to LDAP objects
1.3.6.1.4.1.[PEN].2.[N] - For various LDAP Objects

Where N can be integer representing LDAP attributes, classes, syntax, matching rules etc.

### EBAY specific structure
Considerations
- Internal Usage : Multiple teams within eBay manage various LDAP objects
- External Contributions: Opensource and external contributions need identifiecation


To allow for extensibility following structure is adopted:
1.3.6.1.4.1.[EBAY PEN].[Org Unit].[PROTOCOL].[N]
Where:
- EBAY PEN = 55805
- Org Unit = [1-50: EBAY Internal, 51-100: Reserved, 101+ External ]
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
