# Identity Mapping

TrustFabric allows identity creation via mappings. This allows easier and flexible adoption of identity representation in your environment.

## Mapping Identities in LDAP

LDAP mapping is needed if your organization uses LDAP directories or IdM/IAM systems backed by LDAP directories. LDAP based identity representation makes it easier to achieve federation for application identities and can also be leveraged to build application catalog.
Please refer to [LDAP Schema section](./LdapSchema.md) for LDAP based identity modelling.

## Mapping Identities in Kubernetes

In Kubernetes multiple options exist for implementing the mappings: 

1. Simple mapping: Leverage Clusters, Namespacesand , Deployments (including `deplpoyment`, `rc`, `statefulset` etc.) values to represent the workspace, application and deployment respectively. This model does have lot of limitations for a large deployment with multiple clusters. But for a smaller deployment, this may work well.
1. Leverage `Workspace`, `Application` and `AppInstance` kinds introduced in the [Kubernetes Models](./K8S.md). CRDs and controller implementation is needed to make this mapping work along with few admission control web-hooks and federation. This approach is recommended only for large deployments (with multiple clusters)
1. Other mappings using  combination of Kubernetes constructs (Cluster, Namespace, Deployment, etc) along with CRDs (environment, VPC etc) can be used to model application model as well. 

**Important**: TrustFabric is a federated implementation for application and user identities. Based on your implementation, additional controls may be required to implement unique identities across a multi-cluster Kubernetes infrastructure