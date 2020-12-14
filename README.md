# Drupal kubernetes deployment example

Start the kubernetes cluster with minikube
```
minikube start --cpus 2 --memory 4096
```
Map  Docker Daemon from minikube to the local environment
```
eval $(minikube docker-env)
```
Build Docker image
```
docker build -t drupal:local .
```
Create a name space:
```
# Create a namespace for the Drupal deployment.
kubectl create namespace d8
```
Deploy Mysql 
```
# Create the MySQL Deployment.
kubectl apply -f k8s/db.yml
```
Deploy Drupal 9
```
# Create the Drupal (Apache + PHP) Deployment.
kubectl apply -f k8s/drupal.yml
```
## Accessing the Drupal site

After the Drupal deployment is complete, you can see access it via:

```
# In Minikube, this will open the URL directly.
minikube service -n drupal drupal

# In other clusters, get the service to get the NodePort.
kubectl get service -n drupal drupal
```

## Cleaning up

After testing you can clean the deployments by deleting the namespace

```
kubectl delete namespace d8
```

To get rid of the Volumes :

```
kubectl get pv | grep Released | awk '$1 {print$1}' | while read vol; do kubectl delete pv/${vol}; done
```
