# Application Identity - Kubernetes CRD Model

## Reference

1. [Kubernetes CRD Reference schema](../resources/tfschema.yaml)

## Model

Kubernetes model provided for identity representation is for reference only and may change.
Kubernetes CRD models include following objects:

1. `Workspace` kind represents the workspace type. 
2. `Application` kind represents the component that is `part-of` the `workspace`
3. `AppInstance` kind represents the deployment (links to Kubernetes `deployment`, `statefulset`, `rc` etc.) that s `instance-of` the `Application` and has and `environment` identifier.

### Examples

```
---
apiVersion: ebay/v1alpha1
kind: Workspace
metadata:
  name: checkout  
spec:
  workspaceId: "26BA471D-EF21-446F-B299-90CA2E110F61"
  source: "http://git.acme.com/checkout"
  tracking: "http://jira.acme.com/checkout"
  groupRef:
    kind: Group
    name: checkout_admin
  owner: jdoe

---
apiVersion: ebay/v1alpha1
kind: Application
metadata:
  name: checkoutdb
spec:
  component: "database"
  applicationId: "8a3c8772-67fd-4fcb-a922-1e0e176e66aa"
  part-of: "checkout"
  dataClassification:
  - PUBLIC

---
apiVersion: extensions/v1beta1
kind: AppInstance
metadata:
  name: mysql-7886
spec:
  instanceId: "c2a16ae7-45a5-4c55-b1cf-2bc9da692c7c"
  environment: production
  cluster: cluster-32
  instance-of: checkoutdb

```