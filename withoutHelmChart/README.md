# Setting Jenkins master-slave on Kubernetes cluster without using Helm chart

The Setup uses custom **Jenkins image**  for master which is pulled while creation of deployments from docker repo

## Step1. Create and deploy "deployment object" for jenkins-master node 

```

```

## Step2. Installation of **jenkins Using helm**
Main ingredient of the setup is **values.yaml**
```
helm install -n jenkinscicd stable/jenkins -f values.yaml --version 0.16.5 --wait
```

## Step3. Configuring the ClusterIP svc to NodePort

```
kubectl get all
kubectl get svc cd-jenkins -o yaml | tee cd-jenkins.yaml
 NodePort
kubectl apply -f cd-jenkins.yaml

kubectl get svc

minikube service cd-jenkins
```

## Step4. Getting jenkins-admin password from **secret**

```
printf $(kubectl get secret jenkinscicd -o jsonpath="{.data.jenkins-admin-password}" | base64 --decode);echo
```

## Step5. Flushing all steps
```
kubectl get events
kubectl get all
helm list
helm del <chartname> --purge
helm get values jenkinscicd
```