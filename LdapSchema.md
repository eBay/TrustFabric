# LDAP Schema for Application Identities

**Important**: Please read following references:
- [LDAP Schema Reference](./LDAPRef.md)
- [Identity Model](./Identity.md)
- [LDIF for the reference schema model](./resources/tfschema.ldif)

## DN Naming requirements
TrustFabric is a very flexible specification for object representation. Any schema representation that meets naming requirements below can be used.
| Entity | RDN Attributes | Description |
| --- | ----------- | ----|
| Business Function/Workspace | `o` (organization) | Business function name e.g. cart `o=cart`|
| Application | `ou` (Organizational unit) | Application component e.g. cart database `ou=cart-mysql`|
| Application Instance/Deployment | `cn` + `l` | Name and location of the deployment e.g. `cn=cart-mysql-us + l=production` |

## Reference Schema Model

Schema provided here is for reference. You can use any representation that matches the DN naming requirement and works for your requirements.

### Attributes
#### Workspace ID
It is the identifier for the Business Function/workspace. Typically a UUID value and may be different from human readable value which can be represented using `o` attribute.
```ldif
( 1.3.6.1.4.1.55988.1.2.3.601 NAME 'tf-workspace-id'
	EQUALITY caseIgnoreMatch
	SUBSTR caseIgnoreSubstringsMatch
	SYNTAX 1.3.6.1.4.1.1466.115.121.1.15 
	DESC 'Identifier (UUID/GUID) for the Business Function/workspace'
)
```
#### Application ID
Identifier for the application. Typically a UUID value and may be different from human readable value which can be represented using `ou` attribute.
```ldif
( 1.3.6.1.4.1.55988.1.2.3.602 NAME 'tf-application-id'
	EQUALITY caseIgnoreMatch
	SUBSTR caseIgnoreSubstringsMatch
	SYNTAX 1.3.6.1.4.1.1466.115.121.1.15 
	DESC 'Application Identifier (UUID/GUID)'
)
```

#### Application Instance ID
Identifier for the deployment or the instance of the application in a specific zone/lifecycle. Typically a UUID value and may be different from human readable value which can be represented using `cn` attribute.
```ldif
( 1.3.6.1.4.1.55988.1.2.3.603 NAME 'tf-app-instance-id'
	EQUALITY caseIgnoreMatch
	SUBSTR caseIgnoreSubstringsMatch
	SYNTAX 1.3.6.1.4.1.1466.115.121.1.15 
	DESC 'Application instance (deployment) Identifier (UUID/GUID)'
)
```
#### Source Code Repository
Source code repository URI.
```ldif
( 1.3.6.1.4.1.55988.1.2.3.611 NAME 'tf-source-repository'
	EQUALITY caseExactString
	SUBSTR caseExactSubstringsMatch
	SYNTAX 2.5.5.3 
	DESC 'Source code repository URI'
)
```
#### Issue Repository
URI for the project in the issue tracking system
```ldif
( 1.3.6.1.4.1.55988.1.2.3.612 NAME 'tf-issue-repository'
	EQUALITY caseExactString
	SUBSTR caseExactSubstringsMatch
	SYNTAX 2.5.5.3 
	DESC 'Issue tracking system/repository URI'
)
```
#### Software repository
URI for the software download repository or container image registry (based on type of the software) for deployment.
```ldif
( 1.3.6.1.4.1.55988.1.2.3.613 NAME 'tf-software-repository'
	EQUALITY caseExactString
	SUBSTR caseExactSubstringsMatch
	SYNTAX 2.5.5.3 
	DESC 'Software download repository (PKG/Container/ISO repo) URI'
)
```
#### Software Image
URI for the software image/download for a specific version.
```ldif
( 1.3.6.1.4.1.55988.1.2.3.614 NAME 'tf-software-image'
	EQUALITY caseExactString
	SUBSTR caseExactSubstringsMatch
	SYNTAX 2.5.5.3 
	DESC 'Software Image URI for a specific version'
)
```
#### Version
String representing a version number.
```ldif
( 1.3.6.1.4.1.55988.1.2.3.615 NAME 'tf-version'
	EQUALITY caseIgnoreMatch
	SUBSTR caseIgnoreSubstringsMatch
	SYNTAX 1.3.6.1.4.1.1466.115.121.1.15 
	DESC 'Version number string'
)
```

#### Creator
User ID who created the resource.
```ldif
(1.3.6.1.4.1.55988.1.2.3.621 NAME 'tf-creator'
	EQUALITY caseIgnoreMatch
	SUBSTR caseIgnoreSubstringsMatch
	SYNTAX 1.3.6.1.4.1.1466.115.121.1.15 
	DESC 'UID of the creator'
)
```

#### Maintainers
Group ID that contains authorized maintainer of the resource.
```ldif
( 1.3.6.1.4.1.55988.1.2.3.622 NAME 'tf-maintainers'
	EQUALITY caseIgnoreMatch
	SUBSTR caseIgnoreSubstringsMatch
	SYNTAX 1.3.6.1.4.1.1466.115.121.1.15 
	DESC 'Group ID of the maintainers'
)
```

#### Approvers
Group ID that contains approvers of the resource and resource roles.
```ldif
( 1.3.6.1.4.1.55988.1.2.3.623 NAME 'tf-admins'
	EQUALITY caseIgnoreMatch
	SUBSTR caseIgnoreSubstringsMatch
	SYNTAX 1.3.6.1.4.1.1466.115.121.1.15 
	DESC 'Group ID of the approvers'
)
```

## Object classes
#### Workspace
Workspace object-class requires `o` (`Organization` attribute)  as a required attribute (required for RDN). In addition, `tf-workspace-id` can be provided which may represent associated name/identifier in the PaaS/Catalog system.
```ldif
( 1.3.6.1.4.1.55988.1.2.4.101 NAME 'tf-workspace'
	SUP top
	STRUCTURAL
	MUST ( o	
	)
	May ( tf-workspace-id $ displayName $ seeAlso $ description $
		tf-source-repository $ tf-issue-repository
		tf-creator $ tf-maintainers $ tf-approvers
	)
)
```
#### Application
Application object-class requires `ou` as must attribute and as `RDN` attribute. The `tf-application-id` attribute may be provided, which can be associated application name/identifier in PaaS/Catalog system.
```ldif
( 1.3.6.1.4.1.55988.1.2.4.102 NAME 'tf-application'
	SUP top
	STRUCTURAL
	MUST ( ou
	)
	May ( tf-application-id $ displayName $ seeAlso $ description $
		tf-source-repository $ tf-software-repository
		$ tf-issue-repository $ tf-creator $ tf-maintainers
		$ tf-approvers
	)
)
```
#### Application Instance
App-Instance object class represents the deployment of the application in a zone/lifecycle. The `cn` attribute should be used to represent the deployment identifier as a human readable name. In addition, the location attribute `l` represents the zone/lifecycle of the deployment and is also a must attribute as `cn`. RDN is a composite RDN of `cn` and `l` so it should be a unique combination. 
```ldif
( 1.3.6.1.4.1.55988.1.2.4.103 NAME 'tf-app-instance'
	SUP top
	STRUCTURAL
	MUST ( cn $ l $ tf-version
	)
	MAY ( tf-app-instance-id $ displayName $ seeAlso $ description $
		tf-software-repository $ tf-software-image
		$ tf-issue-repository $ tf-creator $ tf-maintainer
        $ tf-approvers
	)
)
```

