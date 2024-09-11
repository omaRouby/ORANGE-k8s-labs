Here are the answers to the questions about Kubernetes namespaces:

### Kubernetes Namespaces Questions and Answers

#### 1. What is a namespace in Kubernetes, and why is it used?
A **namespace** in Kubernetes is a way to divide cluster resources between multiple users. Namespaces are intended for use in environments with many users spread across multiple teams or projects. They help to organize resources in a cluster by isolating groups of resources, like pods, services, and deployments, and providing a scope for names.

Namespaces are used to:
- Separate environments (e.g., development, testing, production).
- Provide scope for resource names.
- Apply resource quotas, access controls, and policies.

#### 2. How do you create a new namespace in Kubernetes using the `kubectl` command?
**Command:**
```bash
kubectl create namespace <namespace-name>
```
For example, to create a namespace called `dev-environment`:
```bash
kubectl create namespace dev-environment
```

#### 3. How can you list all namespaces in a Kubernetes cluster?
**Command:**
```bash
kubectl get namespaces
```
or
```bash
kubectl get ns
```

#### 4. What is the default namespace in Kubernetes? What happens if you do not specify a namespace when deploying a resource?
The **default namespace** in Kubernetes is named `default`. If you do not specify a namespace when deploying a resource, it is created in the `default` namespace.

#### 5. How do you delete a namespace in Kubernetes? What happens to the resources within it?
**Command:**
```bash
kubectl delete namespace <namespace-name>
```
When you delete a namespace, all resources within that namespace (e.g., pods, services, deployments) are also deleted.

#### 6. How can you switch between namespaces while using the `kubectl` command?
You can specify the namespace using the `--namespace` flag with any `kubectl` command. For example:
```bash
kubectl get pods --namespace=<namespace-name>
```
Alternatively, you can set a context to use a specific namespace:
```bash
kubectl config set-context --current --namespace=<namespace-name>
```

#### 7. How do you create a Kubernetes deployment in a specific namespace?
**Command:**
```bash
kubectl create deployment <deployment-name> --image=<image-name> -n <namespace-name>
```
For example, to create a deployment named `nginx-deployment` in the `dev-environment` namespace:
```bash
kubectl create deployment nginx-deployment --image=nginx -n dev-environment
```

#### 8. Can two different namespaces have resources with the same name? Explain your answer.
Yes, two different namespaces can have resources with the same name because namespaces provide scope for resource names. Resources in different namespaces do not conflict, as each namespace acts as a virtual cluster with its own set of resources.

#### 9. How can you check the resource quotas and limits for a specific namespace?
**Command:**
```bash
kubectl get resourcequota -n <namespace-name>
```
To get detailed information, you can describe the resource quota:
```bash
kubectl describe resourcequota <resourcequota-name> -n <namespace-name>
```

#### 10. How do you configure a `kubectl` context to always use a specific namespace by default?
**Command:**
```bash
kubectl config set-context --current --namespace=<namespace-name>
```

#### 11. Create a YAML file to define a new namespace called `dev-environment`. Deploy it using `kubectl`.

**YAML file (`dev-environment-namespace.yaml`):**
```yaml
apiVersion: v1
kind: Namespace
metadata:
  name: dev-environment
```

**Command to deploy it:**
```bash
kubectl apply -f dev-environment-namespace.yaml
```

#### 12. Write a command to deploy a pod named `test-pod` running the `nginx` image into a namespace called `testing`.
**Command:**
```bash
kubectl run test-pod --image=nginx -n testing
```

#### 13. List all the pods running in a namespace called `production`.
**Command:**
```bash
kubectl get pods -n production
```
