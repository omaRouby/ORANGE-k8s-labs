
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

#### 2. How many PODs are DESIRED in the new-replica-set?
There are 4 desired PODs in the new-replica-set, as defined by the `replicas` field.

#### 3. What is the image used to create the pods in the new-replica-set?
The image used is `busybox777`.

#### 4. How many PODs are READY in the new-replica-set?
The number of READY PODs would be 0 because the image `busybox777` likely does not exist or is incorrect.

#### 5. Why do you think the PODs are not ready?
The PODs are not ready because the `busybox777` image is probably not available in the Docker registry, leading to an `ImagePullBackOff` or `ErrImagePull` error.

#### 6. Delete any one of the 4 PODs. How many pods now?
After deleting one pod, there will still be 4 pods.

#### 7. Why are there still 4 PODs, even after you deleted one?
ReplicaSets ensure that the desired number of replicas is maintained. When one pod is deleted, the ReplicaSet controller creates a new one to match the desired state of 4 replicas.

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

---
