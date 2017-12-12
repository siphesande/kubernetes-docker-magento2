# kubernetes-docker-magento2
Kubernetes Engine, Docker, Magento 2

Getting Satarted
```
- Clone this ^ repository "aka" repo
```
Prerequisites:
```- Install Docker: https://www.docker.com  
   - $ brew install docker
   - this will help you get started https://kubernetes.io/docs/tutorials/stateless-application/hello-minikube/

```
Start Magento 2 with Docker Compose
On your command line
```
$ docker-compose up -d
$ docker ps
$ docker logs -f CONTAINER_ID
To see your Magento visit http://localhost:8000
```
Start Magento 2 with Kubernetes
```
- By now, you have installed kubectl and minikube, if not please check here:https://kubernetes.io/docs/tutorials/stateless-application/hello-minikube/
- If you are using Google Cloud, make sure that you have done these:
  1. Installed Google Cloud SDK
  2. Created a Project
  3. Created a cluster - $ gcloud container clusters create cluster_name
                       - $ gcloud config set container/cluster cluster_name  

  4. Connect to the cluster - $ gcloud container clusters get-credentials cluster_name --zone us-central1-a --project project_name
                            - $ kubectl proxy
                            - Open the Dashboard interface > http://localhost/ui

```                                                                
MySQL deployment to Kubernetes and Set up

First step to deploy MySQL is to create a Kubernetes Secret to store the password for the database. To create a Secret named mysql, run the following command (and replace YOUR_PASSWORD with a passphrase of your choice):
```
$ kubectl create secret generic mysql --from-literal=password=YOUR_PASSWORD
```
Deploy mysql
```
$ kubectl create -f mysql.yaml
```
Create MySQL service
The next step is to create a Service to expose the MySQL container and make it accessible from the magento2 container you are going to create.
```
$ kubectl create -f mysql-service.yaml
```

Magento 2 Set up and deployment to Kubernetes
```
$ kubectl create -f magento2.yaml
```
To expose your Magento2 application to traffic from the internet using a load balancer, you need a Service with type:LoadBalancer
```
$ kubectl create -f magento2-service.yaml
```
Once the service is created, we can get the externally accessible IP address by listing all the services:
```
$ kubectl get services
```

From this, you can now access your application by visiting the External IP address on port 80
```
http://104_________:80/
```
Cleaning up
``` $ kubectl delete secret  mysql
    $ kubectl delete deployment -l app=magento2
    $ kubectl delete service -l app=magento2
    $ gcloud compute disks delete mysql-disk magento-disk

    You can also do this by using kubernetes ui(Dashboard) and GCP ui
```
Usefull links:
```
- https://kubernetes.io/docs/tutorials/stateless-application/hello-minikube/
- https://kubernetes.io/docs/concepts/workloads/controllers/deployment/
- https://github.com/alexcheng1982/docker-magento2
- https://scotch.io/tutorials/google-cloud-platform-i-deploy-a-docker-app-to-google-container-engine-with-kubernetes
- https://cloud.google.com/kubernetes-engine/docs/tutorials/persistent-disk
- https://kubernetes.io/docs/tutorials/stateful-application/mysql-wordpress-persistent-volume/
```
