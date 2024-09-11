
---

### Lab 2 in Kubernetes

#### 1. Create a ReplicaSet using the below YAML:
```yaml
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: new-replica-set
  namespace: default
spec:
  replicas: 4
  selector:
    matchLabels:
      name: busybox-pod
  template:
    metadata:
      labels:
        name: busybox-pod
    spec:
      containers:
      - command:
        - sh
        - -c
        - echo Hello Kubernetes! && sleep 3600
        image: busybox777
        imagePullPolicy: Always
        name: busybox-container
```
**Command:**
```bash
kubectl apply -f my-rs.yaml
```
![ReplicaSet Creation](https://github.com/omaRouby/ORANGE-k8s-labs/blob/main/lab-2/images/21.png)

#### 2. How many PODs are DESIRED in the new-replica-set?
There are 4 desired PODs in the new-replica-set, as defined by the `replicas` field.

**Command:**
```bash
kubectl get rs new-replica-set
```
![Desired Pods](https://github.com/omaRouby/ORANGE-k8s-labs/blob/main/lab-2/images/22.png)

#### 3. What is the image used to create the pods in the new-replica-set?
The image used is `busybox777`.

**Command:**
```bash
kubectl describe rs new-replica-set
```
![Image Used](https://github.com/omaRouby/ORANGE-k8s-labs/blob/main/lab-2/images/23.png)

#### 4. How many PODs are READY in the new-replica-set?
The number of READY PODs would be 0 because the image `busybox777` likely does not exist or is incorrect.

**Command:**
```bash
kubectl get pods
```
![Ready Pods](https://github.com/omaRouby/ORANGE-k8s-labs/blob/main/lab-2/images/24.png)

#### 5. Why do you think the PODs are not ready?
The PODs are not ready because the `busybox777` image is probably not available in the Docker registry, leading to an `ImagePullBackOff` or `ErrImagePull` error.

**Command:**
```bash
kubectl describe pod <pod-name>
```

#### 6. Delete any one of the 4 PODs. How many pods now?
After deleting one pod, there will still be 4 pods.

**Commands:**
```bash
kubectl delete pod <pod-name>
kubectl get pods
```

#### 7. Why are there still 4 PODs, even after you deleted one?
ReplicaSets ensure that the desired number of replicas is maintained. When one pod is deleted, the ReplicaSet controller creates a new one to match the desired state of 4 replicas.

**Command:**
```bash
kubectl get rs new-replica-set
```
![ReplicaSet Status](https://github.com/omaRouby/ORANGE-k8s-labs/blob/main/lab-2/images/24.png)

#### 8. Create a ReplicaSet using the below YAML (fixed version):

There is an issue with the file, so try to fix it.

**Original YAML (with issue):**
```yaml
apiVersion: v1  # Incorrect apiVersion
kind: ReplicaSet
metadata:
  name: replicaset-1
spec:
  replicas: 2
  selector:
    matchLabels:
      tier: frontend
  template:
    metadata:
      labels:
        tier: frontend
    spec:
      containers:
      - name: nginx
        image: nginx
```

**Fixed YAML:**
```yaml
apiVersion: apps/v1  # Correct apiVersion for ReplicaSet
kind: ReplicaSet
metadata:
  name: replicaset-1
spec:
  replicas: 2
  selector:
    matchLabels:
      tier: frontend
  template:
    metadata:
      labels:
        tier: frontend
    spec:
      containers:
      - name: nginx
        image: nginx
```
**Command:**
```bash
kubectl apply -f fixed.yaml
```
![Fixed ReplicaSet](https://github.com/omaRouby/ORANGE-k8s-labs/blob/main/lab-2/images/25.png)

---
