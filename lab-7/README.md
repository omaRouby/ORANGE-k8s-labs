### Understanding RBAC in Kubernetes

**1. What is RBAC in Kubernetes, and why is it important?**

RBAC (Role-Based Access Control) in Kubernetes is a method for regulating access to resources within a Kubernetes cluster. It allows administrators to define roles and permissions for users or service accounts, controlling what actions they can perform on which resources. This is crucial for ensuring security, managing access permissions, and enforcing the principle of least privilege within the cluster.

**2. What are the main components of RBAC in Kubernetes? Describe the purpose of each.**

- **Role**: Defines a set of permissions within a specific namespace. It specifies what resources can be accessed and what actions can be performed on those resources. Roles are used to grant permissions to users or service accounts within a namespace.

- **ClusterRole**: Similar to a Role, but it defines permissions across the entire cluster. ClusterRoles can be used to grant permissions that are not limited to a specific namespace or apply to all namespaces.

- **RoleBinding**: Associates a Role with a user or set of users within a specific namespace. It binds the defined permissions of a Role to specific subjects (users, groups, or service accounts) in that namespace.

- **ClusterRoleBinding**: Associates a ClusterRole with a user or set of users across the entire cluster. It binds the permissions defined in a ClusterRole to subjects at the cluster level.

**3. How do Roles differ from ClusterRoles in Kubernetes?**

- **Role**: Defines permissions within a specific namespace. It is scoped to a single namespace, meaning that it applies only to resources within that namespace.

- **ClusterRole**: Defines permissions at the cluster level or for resources that are not namespaced (e.g., nodes, cluster-wide resources). It can be used across all namespaces or for resources that exist outside of namespaces.

**4. What is a RoleBinding in Kubernetes, and how does it differ from a ClusterRoleBinding?**

- **RoleBinding**: Binds a Role to specific users or service accounts within a particular namespace. It allows those users or accounts to perform actions defined by the Role in that namespace only.

- **ClusterRoleBinding**: Binds a ClusterRole to users or service accounts across the entire cluster. It grants permissions defined by the ClusterRole to subjects at the cluster level, applicable across all namespaces.

**5. How can you list all the Roles and RoleBindings in a specific namespace?**

To list all Roles and RoleBindings in a specific namespace, you can use the following commands:

```sh
kubectl get roles -n <namespace>
kubectl get rolebindings -n <namespace>
```

Replace `<namespace>` with the name of your namespace.

### RBAC YAML Configurations

**6. How do you create a Role that allows a user to read secrets only in a specific namespace? Provide a YAML example.**

**Role YAML**:

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  name: secret-reader
  namespace: <namespace>
rules:
- apiGroups: [""]
  resources: ["secrets"]
  verbs: ["get", "list"]
```

Replace `<namespace>` with the target namespace.

**7. Explain how ClusterRoleBindings can be used to grant permissions across the entire Kubernetes cluster.**

ClusterRoleBindings are used to bind a ClusterRole to users or service accounts at the cluster level. This means that the permissions defined in the ClusterRole are applied across all namespaces in the cluster. For example, a ClusterRoleBinding can be used to grant cluster-wide administrative access or read-only access to all resources in the cluster.

**8. What are Subjects in RBAC, and what types of subjects can be used in RoleBindings or ClusterRoleBindings?**

Subjects are entities that can be assigned roles in RoleBindings or ClusterRoleBindings. They can be:
- **User**: A human user or identity in the system.
- **Group**: A group of users.
- **ServiceAccount**: A service account used by applications running in Pods.
- **ExternalName**: An external identity provider.

**9. How can you check the permissions of a particular user or service account in a Kubernetes cluster?**

To check the permissions of a user or service account, you can use the `kubectl auth can-i` command:

```sh
kubectl auth can-i <verb> <resource> --as <user>
```

Replace `<verb>` with the action (e.g., `get`, `list`), `<resource>` with the resource type (e.g., `pods`), and `<user>` with the user or service account name.

**10. What is the significance of the aggregate-to-admin, aggregate-to-edit, and aggregate-to-view labels in Kubernetes RBAC?**

These labels are used to aggregate roles to provide common sets of permissions:
- **aggregate-to-admin**: Includes permissions for administrative tasks.
- **aggregate-to-edit**: Includes permissions for modifying resources.
- **aggregate-to-view**: Includes permissions for viewing resources.

They simplify role management by allowing roles to be combined to create more comprehensive roles.

**11. How does Kubernetes RBAC integrate with external identity providers (e.g., LDAP, OIDC)?**

Kubernetes RBAC can integrate with external identity providers via authentication mechanisms like OIDC (OpenID Connect). This allows Kubernetes to use external systems for user authentication, while RBAC manages authorization within the cluster based on the authenticated identities.

**12. How do you troubleshoot RBAC permission errors in Kubernetes? What are some common issues?**

- **Verify Role and RoleBinding/ClusterRoleBinding**: Check that the roles and bindings are correctly defined and associated with the right subjects.
- **Use `kubectl auth can-i`**: Test specific permissions to identify what actions a user or service account is allowed to perform.
- **Check for Misconfigured Roles**: Ensure that the roles have the correct verbs and resources defined.
- **Review RBAC Logs**: Examine logs for detailed error messages and context.

**13. Explain how to grant temporary elevated privileges to a user in Kubernetes. What are the security implications?**

To grant temporary elevated privileges, you can create a Role or ClusterRole with the desired permissions and bind it to the user or service account for a short period. The security implications include potential exposure of sensitive resources or the risk of misuse if the elevated permissions are not closely monitored.

**14. What is the difference between using RBAC and Kubernetes' native ServiceAccount permissions for pods?**

RBAC provides a more granular and flexible way to manage permissions for users and service accounts. ServiceAccount permissions are more basic and tied to the specific Pods that use them. RBAC allows for more complex and detailed access control policies across the cluster.

**15. How can you restrict access to certain Kubernetes API groups or resources using RBAC? Provide a YAML example.**

You can restrict access by specifying API groups and resources in Role or ClusterRole definitions.

**YAML Example**:

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  name: restricted-access
rules:
- apiGroups: [""]
  resources: ["pods"]
  verbs: ["get", "list"]
- apiGroups: ["apps"]
  resources: ["deployments"]
  verbs: ["get"]
```

**YAML Definitions for RBAC Tasks**

**16. Write a YAML definition for a ClusterRole that allows listing all pods in any namespace.**

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  name: pod-lister
rules:
- apiGroups: [""]
  resources: ["pods"]
  verbs: ["list"]
```

**17. Create a Role that allows a user to create, update, and delete deployments only within the dev namespace.**

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  name: deployment-manager
  namespace: dev
rules:
- apiGroups: ["apps"]
  resources: ["deployments"]
  verbs: ["create", "update", "delete"]
```

**18. Set up a RoleBinding that assigns the view role to a user named john in the testing namespace.**

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: view-binding
  namespace: testing
subjects:
- kind: User
  name: john
  apiGroup: rbac.authorization.k8s.io
roleRef:
  kind: Role
  name: view
  apiGroup: rbac.authorization.k8s.io
```

**19. Deploy a ClusterRoleBinding that grants the edit role to a service account named developer in all namespaces.**

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: developer-edit-binding
subjects:
- kind: ServiceAccount
  name: developer
  namespace: default
roleRef:
  kind: ClusterRole
  name: edit
  apiGroup: rbac.authorization.k8s.io
```

**20. Write a command to check if a user named alice has permission to delete pods in the production namespace.**

```sh
kubectl auth can-i delete pods --as alice -n production
```

**21. Write a YAML definition for a Role named `pod-executor` in the `ci-cd` namespace that allows creating, listing, and executing Pods. Bind this role to a service account named `pipeline-sa` using a RoleBinding.**

**Role YAML**:

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  name: pod-executor
  namespace: ci-cd
rules:
- apiGroups: [""]
  resources: ["

pods"]
  verbs: ["create", "list", "exec"]
```

**RoleBinding YAML**:

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: pod-executor-binding
  namespace: ci-cd
subjects:
- kind: ServiceAccount
  name: pipeline-sa
  namespace: ci-cd
roleRef:
  kind: Role
  name: pod-executor
  apiGroup: rbac.authorization.k8s.io
```

**22. Create a Role named `persistent-volume-access` in the `storage` namespace that grants permissions to create and delete PersistentVolumeClaims. Assign this role to a service account named `storage-admin` using a RoleBinding.**

**Role YAML**:

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  name: persistent-volume-access
  namespace: storage
rules:
- apiGroups: [""]
  resources: ["persistentvolumeclaims"]
  verbs: ["create", "delete"]
```

**RoleBinding YAML**:

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: persistent-volume-access-binding
  namespace: storage
subjects:
- kind: ServiceAccount
  name: storage-admin
  namespace: storage
roleRef:
  kind: Role
  name: persistent-volume-access
  apiGroup: rbac.authorization.k8s.io
```

**23. Create a ClusterRole named `readonly-cluster` that grants read-only access to all resources across the cluster. Then, create a ClusterRoleBinding that assigns this role to a user named alice.**

**ClusterRole YAML**:

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  name: readonly-cluster
rules:
- apiGroups: [""]
  resources: ["pods", "services", "endpoints", "namespaces"]
  verbs: ["get", "list"]
- apiGroups: ["apps"]
  resources: ["deployments", "statefulsets"]
  verbs: ["get", "list"]
```

**ClusterRoleBinding YAML**:

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: readonly-cluster-binding
subjects:
- kind: User
  name: alice
  apiGroup: rbac.authorization.k8s.io
roleRef:
  kind: ClusterRole
  name: readonly-cluster
  apiGroup: rbac.authorization.k8s.io
```

