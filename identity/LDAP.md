# Application Identity - LDAP Model

## Reference

1. [LDAP Reference schema](../resources/tfschema.ldif)

## Model

Application identities can be modelled and stored in LDAP servers. The reference schema provided here consists of three object classes:

1. `tf-workspace` represents the workspace object class. This `objectclass` has `o` (organization) attribute which can be used to represent human readable (unique) value of the workspace. In addition, `tf-workspace-id` is UUID representing the workspace-id. In addition, multiple optional attributes e.g. source repository, description etc can be defined.
1. `tf-application` represents the application (or component) object class. This `objectclass` has `ou` (Org unit) attribute, which can be used to represent human readable value of the application. This value must be unique at the workspace level. If your model does not use workspace object, both `ou` must be unique value. In addition `tf-application-id` is an UUID that must be defined. Other optional attributes can be defined as well.
1. `tf-app-instance` represents the AppInstance object class. This `objectclass` has `cn` (common name) and `l` (location) attributes which map to apo-instance human readable name and environment respectively. In addition UUID value for the AppInstance `tf-app-instance-id` must be defined. The `cn` attribute must be unique under the application object and need not be globally unique value.
