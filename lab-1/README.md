Here's the paragraph formatted for a GitHub README file, with placeholders for the screenshot links:

---

### Kubernetes Commands and Output Clarifications

1. **How many pods exist on the system?**  
   No pods exist.

2. **How many Nodes exist on the system?**  
   There are 2 Nodes on the system based on the output: `controlplane` and `node01` listed.

3. **Create a new pod with the nginx image. Image name: nginx**  
   ```bash
   kubectl run my-pod --image=nginx
   ```

4. **Which nodes are these pods placed on?**  
   The pods are placed on `node01`.
   ![YAML Pod Screenshot](https://github.com/omaRouby/ORANGE-k8s-labs/blob/main/lab-1/images/Screenshot%202024-09-11%20025013.png)

5. **Create pod from the below YAML using `kubectl apply` command**  
   ![YAML Pod Screenshot](https://github.com/omaRouby/ORANGE-k8s-labs/blob/main/lab-1/images/Screenshot%202024-09-11%20030134.png)

6. **How many containers are part of the pod webapp?**  
   The provided YAML defines a pod with 2 containers: `nginx` and `agentx`. However, due to the image pull error for `agentx`, the pod might not be running correctly.

7. **What images are used in the new webapp pod?**  
   The new webapp pod is configured to use two images:
   - `nginx`
   - `agentx`
   ![YAML Pod Screenshot](https://github.com/omaRouby/ORANGE-k8s-labs/blob/main/lab-1/images/Screenshot%202024-09-11%20030156.png)

8. **What is the state of the container `agentx` in the pod webapp?**  
   Due to the image pull error for `agentx`, the container is likely not running or may be in an error state.

9. **Why do you think the container `agentx` in pod webapp is in error?**  
   The logs from `kubectl describe pod webapp` should reveal an error message related to the `agentx` image. The `agentx` image might not exist in the Docker registry.

> **Note**: You used `kubectl delete all --all` to remove all pods, services, and other Kubernetes resources in the default namespace. Be cautious with this command as it can delete unintended resources. Consider using `kubectl get pods` to monitor the status of your pods after creating them.  
> ![Kubernetes Resources Screenshot](https://github.com/omaRouby/ORANGE-k8s-labs/blob/main/lab-1/images/Screenshot%202024-09-11%20030225.png)

---


