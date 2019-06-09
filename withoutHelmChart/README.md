# Setting Jenkins master-slave on Kubernetes cluster without using Helm chart

## Step1. Create and deploy "deployment object" for jenkins-master node 
jenkins-deployments.yaml file contains the name for the deployment,number of pods in deployments,
image to use for pods,ports to expose,it also skips the jenkins initialization steps	
```
kubectl create -f jenkins-deployments.yaml
```


## Step2. Creating two services, one for accessing jenkins master pod UI(NodePort type) and other for jenkins
## tunneling through which jenkins slave pods will connect to master(ClusterIP)

```
kubectl create -f jenkins-svc.yaml jenkins-svc2.yaml
```

## Step3. Creating role for "default" namespace and "default" service account and binding it to through rolebinding
The "role.yaml" file access the pods resources and it has right to create,delete etc the pods
```
kubectl apply -f example-role.yaml 
```

## Important cmds for monitoring and trouble shooting
```
kubectl get pods,po,svc
kubectl get all
kubectl describe pods <pods-name>
kubectl delete pod <pod-name>
kubectl exec -it pod-name
printf $(kubectl get secret jenkinscicd -o jsonpath="{.data.jenkins-admin-password}" | base64 --decode);echo
```

* If all the above objects are deployed correctly and properly,
  while sheduling a jobs on master pod, a new pod for slave will be created 
  and it will be automatically terminated after the build finished. 


  