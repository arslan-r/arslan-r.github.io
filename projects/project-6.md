---
layout: project
type: project
image: images/MachineLearningPicSquare.jpg
title: AWS Project
permalink: projects/awsproject
# All dates must be YYYY-MM-DD format!
date: 2020-04-05
labels:
  - AWS
summary: AWS Project tech and how to.
---

# AWS-Project
Through using AWS materials on their website, I hosted a static website on Amazon S3, converted it to a dynamic one with AWS Fargate, allowed it to store data with DynamoDB, and enabled user registration + authentication through AWS Cognito and REST API. AWS Lambda was further used with user generated data.


This is meant to provide a simple explanation to technologies used in the cloud. This will NOT go into specifics of how to do it, but merely provide an idea of how everything works together. The code for the website is provided by Amazon in their Mythical Mysfits guide, hence there is no code in this repository. Most of the actions taken will be done through the AWS terminal.





Lets start out by hosting a static website on the cloud. Static content, such as HTML, CSS, and media content will be hosted on Amazon S3, which stands for Simple Storage Service. Objects here are stored directly via HTTP.

We will create an S3 bucket and enable the bucket to be used for static website hosting. Amazon S3 buckets, which are like file folders, store objects that consist of data and its descriptive metadata.


This allows data in the bucket to be requested via registered public DNS (Domain Name Service) . However, all buckets created in AWS are set to Private by default. We have to create an S3 Bucket Policy that allows the public to access it. The policies are JSON documents that define which S3 Actions (API calls) are allowed and by whom. (who? whom? Gonna have to reference The Office for this one)



Next, we want our website to integrate with an application back end. We create a microservice for this and host it with AWS Fargate. What is AWS Fargate? What are microservices? Explained at the bottom!



AWS Fargate is a deployment option in amazon elastic container service that allows one to deploy containers without having to manage clusters or servers. 


But what are containers?
Commonly mentioned names are Docker and Kubernetes. You may have heard of them.
Let’s say you have an application you want to push into production. You could deploy the app on some VMs or use Containers. From a purely resource point of view, it takes less resources to launch the app via a container. Compared to launching an entire OS inside your OS for a virtual machine: "containers are far more lightweight: they share the OS kernel, start much faster, and use a fraction of the memory compared to booting an entire OS." -https://cloud.google.com/containers/


For our Mythical Mysfits backend, we will use Python and create a Flask app in a Docker container behind a Network Load Balancer. Flask is a Python framework. A Network Load Balancer (NLB) will distribute the incoming traffic across several servers. The NLB allows us to elastically scale based on demand or if failures occur. These will form the microservice backend for the frontend website. We will be utilizing AWS Fargate and Amazon Elastic Container Service (ECS) for this. The ECS is where we create a "cluster", which refers to a "cluster of servers" that your service containers will be deployed to. AWS Fargate will allow you to deploy your container to a cluster without managing any servers yourself. 


First, we create the core infrastructure, and then the networking infrastructure which we will create in Amazon VPC (Virtual Private Cloud). We will then need to define the permissions the elastic container service (ECS) and our containers will have with AWS Identity and Access Management Roles


We will use AWS CloudFormation to accomplish all of this. This service provisions the resources by declaring it with JSON or YAML files called CloudFormation templates


It’s a bit of a pain in the butt to redeploy code manually whenever there are updates. We want to automate deployments. We will use AWS Code Services to implement Continuous Integration and Continuous Delivery (CD/CI) and build a pipeline that delivers code with AWS CodeBuild. 


Now that it’s easy to deploy features, we will make the site more scalable with Amazon DynamoDB. This is a NoSQL database with fast performance. After its created, make sure its populated and is returning queries. Then we modify the application code to make requests to the DynamoDB. These requests will be made with AWS Python SDK boto3, which provides a way to interact with AWS services via Python code. 


Let’s add some user authentication now. To do certain functions, we want users to register first. Let’s create a User Pool in AWS Cognito. a User Pool is a user directory inside AWS Cognito, which provides authentication, authorization, and user management for web and mobile apps. This allows the users to sign in with social identity providers, such as Google, Facebook, etc. 


For further user actions, we will establish a RESTful API with Amazon API Gateway, which provides common REST API capabilities right out of the box. An API is code that allows programs to communicate with each other. REST stands for REpresentational State Transfer, which simply put is a way to send and receive data between a server and a client.


So we got a database going. Lets use it to gather information which we can later use and improve our site with. AWS Lambda is useful here as it allows data driven apps to respond in real-time to changes in data, shifts in system state, and actions taken by users. Lambda will connect to data stores and process it it for analytics or machine learning inferences. Aaaaand that’s pretty much it. Hope this gave a big picture overview of how a website could be set up on the cloud using AWS. Most of the code credit is given to Amazon, I simply learned and "architected".


MICROSERVICES
[https://raygun.com/blog/what-are-microservices/](https://raygun.com/blog/what-are-microservices/)
PROS


"The biggest pro of microservices architecture is that teams can develop, maintain, and deploy each microservice independently. This kind of single-responsibility leads to other benefits as well. Applications composed of microservices scale better, as you can scale them separately, whenever it’s necessary. Microservices also reduce the time to market and speed up your CI/CD pipeline. This means more agility, too. Besides, isolated services have a better failure tolerance. It’s easier to maintain and debug a lightweight microservice than a complex application, after all."
 
CONS

"As microservices heavily rely on messaging, they can face certain problems. Communication can be hard without using automation and advanced methodologies such as Agile. You need to introduce DevOps tools such as CI/CD servers, configuration management platforms, and APM tools to manage the network. This is great for companies who already use these methods. However, the adoption of these extra requirements can be a challenge for smaller companies."




  
  
