# kubernetes-docker-magento2
Kubernetes Engine, Docker, Magento 2

Gettying Satarted
- Clone the repo..

Start Magento 2 with Docker Compose

On your command line
```
$ docker-compose up -d
$ docker ps
```
Start Magento 2 with Kubernetes

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

Magento 2 Ste up and deployment to Kubernetes
```
$ kubectl create -f magento2.yaml
```
To expose your Mageto2 application to traffic from the internet using a load balancer, you need a Service with type:LoadBalancer
```
$ kubectl create -f magento2-service.yaml
```
Once the service is created, we can get the externally accessible IP address by listing all the services:
```
$ kubectl get services
```

From this, you can now access your application by visiting the External IP address on port 80
```
http://104..........:80/
```
