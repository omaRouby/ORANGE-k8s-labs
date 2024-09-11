Here are the expected answers and the commands for the given Kubernetes tasks, along with placeholders for images:

---

### Kubernetes Deployment Tasks

#### 1. Create a deployment called `my-first-deployment` of image `nginx:alpine` in the default namespace.  
Check to make sure the deployment is healthy.

**Command:**
```bash
kubectl create deployment my-first-deployment --image=nginx:alpine
kubectl get deployments
kubectl rollout status deployment/my-first-deployment
```
![Deployment Creation]([./path-to-your-screenshot-1.png](https://github.com/omaRouby/ORANGE-k8s-labs/blob/main/lab3/images/31.png))

#### 2. Scale `my-first-deployment` up to run 3 replicas.  
Check to make sure all 3 replicas are ready.

**Commands:**
```bash
kubectl scale deployment my-first-deployment --replicas=3
kubectl get deployments
kubectl get pods -l app=my-first-deployment
```
![Scale Up Deployment](https://github.com/omaRouby/ORANGE-k8s-labs/blob/main/lab3/images/31.png)

#### 3. Scale `my-first-deployment` down to run 2 replicas.

**Commands:**
```bash
kubectl scale deployment my-first-deployment --replicas=2
kubectl get deployments
```
![Scale Down Deployment](https://github.com/omaRouby/ORANGE-k8s-labs/blob/main/lab3/images/32.png)

#### 4. Change the image `my-first-deployment` runs from `nginx:alpine` to `httpd:alpine`.

**Commands:**
```bash
kubectl set image deployment/my-first-deployment nginx=httpd:alpine
kubectl rollout status deployment/my-first-deployment
kubectl get deployments
```


#### 5. Delete the deployment `my-first-deployment`.

**Command:**
```bash
kubectl delete deployment my-first-deployment
kubectl get deployments
```


#### 6. Create deployment from the below YAML:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: frontend-deployment
  namespace: default
spec:
  replicas: 4
  selector:
    matchLabels:
      name: busybox-pod
  strategy:
    rollingUpdate:
      maxSurge: 25%
      maxUnavailable: 25%
    type: RollingUpdate
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
        image: busybox888
        imagePullPolicy: Always
        name: busybox-container
```

**Command:**
```bash
kubectl apply -f <filename>.yaml
kubectl get deployments
kubectl get pods
```
![Create Deployment from YAML](https://github.com/omaRouby/ORANGE-k8s-labs/blob/main/lab3/images/32.png)

#### 7. How many ReplicaSets exist on the system now?
There is 1 ReplicaSet associated with the `frontend-deployment`.

**Command:**
```bash
kubectl get rs
```


#### 8. How many PODs exist on the system now?
There are 4 PODs existing on the system as specified in the `replicas` field of the deployment YAML.

**Command:**
```bash
kubectl get pods
```


#### 9. Out of all the existing PODs, how many are ready?
If the image `busybox888` is not available or incorrect, 0 PODs will be ready due to `ImagePullBackOff` or `ErrImagePull`.

**Command:**
```bash
kubectl get pods
```


#### 10. What is the image used to create the pods in the new deployment?
The image used is `busybox888`.

**Command:**
```bash
kubectl describe deployment frontend-deployment
```


#### 11. Why do you think the deployment is not ready?
The deployment is not ready because the image `busybox888` likely does not exist in the Docker registry, causing the pods to fail with an `ImagePullBackOff` error.

**Command:**
```bash
kubectl describe pod <pod-name>
```


---

