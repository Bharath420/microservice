# microservice application 
In this lab, we will try and understand microservices architecture using a simple web application and deploy this web application on Kubernetes platforms in AKS (Azure Kubernetes Service).
We will be using a simple and comprehensive application developed by Docker. 







Sample application: Architecture 
                     
   
The sample voting application provides two interfaces for a user. One is to vote and another interface to show the results. we can see this application is built in the combination of different services, different development tools and multiple different development platforms such as python, Nodejs, .NET, etc..
 
The application consists of various components:
Voting app
Voting app is a kind of web application developed in Python.It provide the user interface to choose between two options a cat and a dog.When you make a selection the vote is stored in Redis.

Redis
 In this case, Redis serves as a database in memory.


Worker 
      The worker application takes the new vote and updates the persistence “Database”  which is at PostgreSQL. Here PostgreSQL will simply have a table with the number of votes for each category and the worker application is written in .NET.
Result of the vote
    The result of the vote is displayed in a web interface which is another web application developed in Nodejs.
Lab: Running sample application in AKS cluster:
Step 1: clone git repository into AZURE CLI:

    git clone

Important Commands: 
  
    “ Kubectl get node”  to see the list of nodes
     “Kubectl get pods”  - to see a list of pods. 
    “ Kubectl get service” to see the service list.
 

Step-2:
 
   So now I'm going to  create these pods and services one by one.I will start with the Voting app.
 kubectl create command -f voting app will create a voting app pod .
   
         kubectl create -f voting-app.yaml

  to check whether it has created or not run get pod command

        kubectl get pod command

Step-3:

Next  to access this voting service from a browser. We need an external service to be created.
Now were going to create the service for the voting app .
   
    kubectl create -f voting-app-service.yml

Now we can list the services using the kubectl get services command.

      kubectl get services 

Once the voting.app service is created we can access the service externally by using external IP address.

Step4 :

Next we’ll create a redis pod, because voting app is dependent on the redis pods.
 
     kubectl create -f  redis-app.yml

  Once the  Redis pod is created.  We can see the status of pod running with redis name.

Use   
     kubectl get pods 
   

Step-5

Now we will create the redis service,so that  redis pod can  access the Services.

     kubectl create -f redis-service.yml
   
 That creates the redis service. 
       
     kubectl get services 

Now we can see the service is running But if we look at the type. it's says cluster IP that indicates that it's an internal service.

Step-6

Now we will create postgres db to store the data of result.

    kubectl create -f postgres-pod.yml

Check the service status using get pod command.

    kubectl get pod

Step-7

 Now we will create the postgres service to enable the other component to access  postgres database.
 
   kubectl create -f postgres-service.yml


Now we can see that, db cluster IP has been created by using get services command line 

   kubectl get services

Step-8

Deploy the worker app.

    kubectl create -f worker-app.yaml


Step-9

Now i am going to create the result app pod.

     kubectl create -f result-app.yaml

Check the pod status 
     
      kubectl get pods


Step-10

Now we will create the result service for making the result app available externally.
   
     kubectl create -f result-app-service.yml

This will be our  final service to create, 

Now we can all the service are up and running by using get pod commands.
Lab: Delete cluster

When the cluster is no longer needed, delete the cluster resources, which deletes all associated resources. This operation can be completed in the Azure portal by selecting the Delete button on the AKS cluster dashboard. Alternatively, the az aks delete command can be used in the Cloud Shell:

“az aks delete --resource-group myResourceGroup --name myAKSCluster --no-wait”


You can then get the current ReplicaSets deployed:

    kubectl get rs
    
You can also check on the state of the replicaset:

   kubectl describe rs/frontend    
 You can also verify that the owner reference of these pods is set to the frontend ReplicaSet. To do this, get the yaml of one of the Pods running:

   kubectl get pods frontend-9si5l -o yaml
   

