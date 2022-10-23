# Setting Jenkins master-slave on GKE cluster using helm



## Step1. Installation of helm 

```
curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3
chmod 700 get_helm.sh
./get_helm.sh
```

## Step2. Installation of jenkins Using helm
```bash
helm repo add jenkins https://charts.jenkins.io
helm repo update
#helm search repo jenkins
helm install jenkins-v1 jenkins/jenkins --namespace=jenkins --create-namespace -f values.yaml --wait

# storing values for future reference
helm get values jenkins-v1
```

## Step3. Getting jenkins-admin password from **secret**
```bash
printf $(kubectl get secret jenkins-v1 -o jsonpath="{.data.jenkins-admin-password}" | base64 --decode);echo

k get svc jenkins-v1 -o jsonpath="{.status.loadBalancer.ingress[].ip}"
```


## Flushing all steps
```
kubectl get events
kubectl get all
helm list
helm uninstall jenkins-v1
helm get values jenkinscicd
```