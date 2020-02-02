# Deployment Architecture of a simple Distributed Web Application using AWS

It's a recomendation of how to do a deployment of a simple distributed application using AWS resources. 

Some premises were assumed during this proposal:

- Variable loads - we don't know the real use of this application, we don't know how many users will used it.
- Both the frontend and the backend applications are stateless, so, no sessions.
- The frontend application is available as a docker image.
- The backend application, composed by a bunch of microservices, are available as a lot of docker image, one for each microservice.


## High Level Network Diagram

![Architecture Diagram](diagram.png)

## AWS EKS

As our application are using containers, including our frontend, is recommended to use 
a container orchestration tool, like K8S - EKS. K8S will give us a lot of capabilities like easy 
AUTOSCALE of our containers as well high availabilty among others.

## JS frontend
In this proposal, some part of static content that don't change frequently, like CSS and IMAGES, (and even some libraries *.JS) are stored in S3 and available through a CDN. 
In other hand, the core dynamic javascript application (main js bundles), are available one or more web application pods, inside the K8S Cluster.
This aproach will take easy the entire update process as well any change in configuration of the aplication can be doing using environment variables.


## Collection of containerized microservices

Inside K8S Cluster. In the diagram, it's represented by backend pods.
As we are working with variable loads, it's recomend to enable autoscale of the pods, something like 2 to 10 pods, (it's a guest only, because to define this number correctly is necessary to do some load tests in our application).

 
## Databases - Postgresql and MongoDB

Both postgresql and Mongo have their own VPC. 
This aproach is used considering the confidence of the database data.
There's no Region replication, it's very expansive and there's no requirement to justify this investiment. 

Both are running as a service, so we don't have to manage the infrastructure by ourselves.
