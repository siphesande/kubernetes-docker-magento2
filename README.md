# kubernetes-docker-magento2
Kubernetes Engine, Docker, Magento 2

Getting Satarted
```
- Clone this ^ repository "aka" repo
```
Prerequisites:
```
   - Download and install Docker($ brew install docker)
   - this will help you get started https://kubernetes.io/docs/tutorials/stateless-application/hello-minikube/

```
Start Magento 2 with Docker Compose
on your command line
```
$ docker-compose up -d
$ docker ps
$ docker logs -f CONTAINER_ID
To see your Magento page visit http://localhost:8000
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
                            - You will get this: Starting to serve on 127.0.0.1:8001 (leave it open and work on the other terminal tap to avoid    terminating the process)
                            - Open the Dashboard interface > http://localhost:8001/ui
  5.Create your persistent disks - $ gcloud compute disks create --size 200GB mysql-disk
                                 - $ gcloud compute disks create --size 200GB magento2-disk
```                                                                
MySQL deployment to Kubernetes and Set up

First step to deploy MySQL is to create a Kubernetes Secret to store the password for the database. To create a Secret named mysql, run the following command (and replace YOUR_PASSWORD with a passphrase of your choice):
```
$ kubectl create secret generic mysql --from-literal=password=YOUR_PASSWORD
```
Deploy MySql
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
phpMyAdmin Set up, deployment to kubernetes and expose
```
$ kubectl create -f phpmyadmin-deploy.yaml
$ kubectl create -f phpmyadmin-service.yaml
 Visit phpMyAdmin dashboard:http://35________:8580
```
Cleaning up
``` $ kubectl delete secret  mysql
    $ kubectl delete deployment -l app=magento2
    $ kubectl delete service -l app=magento2
    (Do the same for mysql and phpmyadmin)
    - You can also delete using kubernetes ui(Dashboard)
    
    If use Google cloud do this:
     $ gcloud compute disks delete mysql-disk magento2-disk
     Delete the cluster, which deletes the resources used by the cluster, including virtual machines, disks, and network resources.
     $ gcloud container clusters delete cluster_name
     Alternately, you can delete the project in its entirety. To do so using the gcloud tool, run:
     $ gcloud projects delete ${PROJECT_ID}

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
