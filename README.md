### Build image

<code>
docker build -t [user_account]/kube-first-app .
docker push [user_account]/kube-first-app
</code>

### Create deployment for kubectl

<code>
kubectl create deployment first-app --image=[user_account]/kube-first-app
</code>

### Check deployment status

<code>
kubectl get deployments
</code>

### Get pods running state

<code>
kubectl get pods
</code>

### Create service to expose port

<code>
kubectl expose deployment first-app --type=LoadBalancer --port=8080
</code>

### Check services types available inside kubernetes

<code>
kubectl get services
</code>

### Access the expose port inside the minikube worker node

<code>
minikube service first-app
</code>

Access the minikube dashboard for pods

<code>
kubectl get pods (See for RESTART column)
minikube dashboard
</code>

### Scale up the services

<code>
kubectl scale deployment/first-app --replicas=3
</code>

Check for scale up pod status
<code>
kubectl get pods
</code>

To scale down the replica set

<code>
kubectl scale deployment/first-app --replicas=1
</code>

Check for scale down pod status
<code>
kubectl get pods
</code>

### Modify source code, rebuild image, push update image to kubernetes

<code>
docker build -t [user_name]/kube-first-app:2 .
kubectl get deployments
docker push [user_name]/kube-first-app:2 // push latest image to docker hub
</code>

To update the image using specified image and update the deployment

<code>
kubectl set image deployment/first-app kube-first-app=[user_name]/kube-first-app:2
</code>

Check the deployment status, and update the deployment rollout

<code>
kubectl get deployments
kubectl rollout status deployment/first-app
</code>

Check for the deployment status from the minikube dashboard for deployment updated image being deployed.

<code>
kubectl get pods
</code>

From the pods apply latest image status, you will see the age for the pod was updated successfully.

### Try rollout invalid image

<code>
kubectl set image deployment/first-app kube-first-app=[user_name]/kube-first-app:3
</code>

Check deployment status

<code>
kubectl rollout status deployment/first-app
</code>

It display "Waiting for deployment "first-app" rollout to finish: 1 old replicas are pending termination..." in console terminal
Check on the "Workload" -> "Pods" in minikube dashboard, it show pending. Dashboard will show the old pods is running and new pod
is pending. It will shutdown the old pods after new pod being deployment succesfully. It ensure the availability of the services
without downtime during update deployment.

Check the kubernetes pod status, it will have show two entries for old and new pod status
<code>
kubectl get pods
</code>

To rollback problematic pod, rollback the deployment for fail pod deployment.
It undo the last deployment status
<code>
kubectl rollout undo deployment/first-app
kubectl get pods
kubectl rollout status deployment/first-app // Console "deployment "first-app" successfully rolled out"
</code>

### View rollout history
To view all the rollout history

<code>
kubectl rollout history deployment/first-app
// To see the details of rollout
kubectl rollout history deployment/first-app --revision=1
kubectl rollout history deployment/first-app --revision=3
kubectl rollout history deployment/first-app --revision=4
</code>

### Revert deployment history
Revert back to the original state of the deployment

<code>
kubectl rollout undo deployment/first-app --to-revision=1
</code>

### Remove deployment and services

<code>
kubectl delete service first-app
kubectl delete deployment first-app
</code>

### Check for all removal process

<code>
kubectl get deployments
kubectl get pods
kubectl get services
kubectl get nodes // For master node control plane
</code>