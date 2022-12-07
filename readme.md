# Streaming video service


* [Description](#description)
* [Functional requirements](#functional-requirements)
* [Non-functional requirements](#non-functional-requirements)
* [Technical and non-technical limitations](#technical-and-non-technical-limitations)
* [Solutions, their advantages and disadvantages](#solutions-their-advantages-and-disadvantages)
* [Storage of configurations](#storage-of-configurations)
* [Communication between services through a queue](#communication-between-services-through-a-queue)
* [Schemes](#schemes)


## Description
This page provides two media solutions to cost-effectively deliver video on-demand and live-streaming content to global audiences using the AWS Cloud. The first one is based on serverless model and microservice architecture, the second - server model and monolith architecture.

## Functional requirements

- broadcast real-time video
- provide video on-demand
- an ability to authorize and register users, using custom authorization and authorization with Google identity provider
- encode and transcode video in real-time
- segment videos and images and store the segments in Amazon S3
- provide RESTful APIs for CRUD management
- support the access of large number of devices

## Non-functional requirements

### The serverless solution

- low latency
- high performance (up to a thousand requests per second)
- high availability and global delivery
- less code
- compatibility
- reliability
- localization

### The server based solution

- low latency
- high performance
- high availability and global delivery
- fault-tolerant system
- quickly deploy, manage, and scale applications without the operational burden of managing infrastructure
- support a blue/green deployment
- compatibility
- reliability
- localization

## Technical and non-technical limitations

### Technical limitations
- maximum file upload size - 256GB
- maximum video length - 10 hours
- maximum video streaming duration - 4 hours
- system must be developed in Node.js at least 16 version
- system must be developed on the fleet of any cloud provider
- video download is available only to authorized users

### Non-technical limitations
- user must have a stable internet connection
- app should not be available to China region

## Solutions, their advantages and disadvantages

1. Using AWS as a cloud provider

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; $+$ cost-effective solution, because you pay only for the compute power, storage, and other resources you use. 

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; $+$ allows to configure services using less code.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; $+$ allows monitor the performance of resources and scale them under certain conditions.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; $+$ an ability to host resources around the world in different availability zones.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; $-$ can be expensive, if developers do not have knowledge about AWS or if you use hign-cost instances.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; $-$ to have specialized knowledge to use.

2. Using S3 storage instaed of DB

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; $+$ no schema.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; $+$ no need to have specialized knowledge to use.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; $+$ the pricing of S3 is cheaper compared to RDS.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; $+$ good for storing video content.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; $+$ good integration with other services.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; $+$ supports versioning.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; $-$ no transactions.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; $-$ no search. 
> search will require an additional solution. for example store video titles in ElasticSearch

$-$ no updates
> can not update a file, just replace it.

3. Using CloudFront

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; $+$ cache content in edge locations and decrease the workload, thus resulting in high availability of applications.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; $+$ simple to use and ensures productivity enhancement.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; $+$ provides high security with the ‘Content Privacy’ feature.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; $+$ facilitates GEO targeting service for content delivery to specific end-users.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; $+$ uses HTTP or HTTPS protocols for quick delivery of content.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; $+$ less expensive than using S3 directly, as it only charges for the data transfer.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; $-$ no disadvantages found.

4. Using Lambda as a serverless solution

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; $+$ no servers to manage

>Lambda runs code on highly available, fault-tolerant infrastructure spread across multiple Availability Zones (AZs) in a single Region, seamlessly deploying code, and providing all the administration, maintenance, and patches of the infrastructure. Lambda also provides built-in logging and monitoring, including integration with Amazon CloudWatch, CloudWatch Logs, and AWS CloudTrail.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; $+$ minimized cost

>Pay only for exactly what you use, you minimize operating cost. None of the price you pay at the end of the month goes towards any unused minutes of server time, as your cost is solely a function of the time your application used.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; $+$ automatic scalability

>an application scales automatically. 

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; $+$ integration

>can set up REST API, notifications, triggers, queue, using Lambda with others services.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; $-$ Has a limits:

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  - function memory allocation - 128 MB to 10,240 MB, in 1-MB increments.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; - function timeout - 900 seconds (15 minutes).

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; - function environment variables - 4 KB, for all environment variables associated with the function, in aggregate.

> But this is not a problem if you use the service suggested below

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; - function burst concurrency - 500 - 3000 (varies per Region).

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; - invocation payload (request and response) - 6 MB each for request and response (synchronous) and 256 KB. (asynchronous).

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; - deployment package (.zip file archive) size - 50 MB (zipped, for direct upload) and 250 MB (unzipped).

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; - container image code package size - 10 GB.

5. Using Elastic Beanstalk as part a server solution

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; $+$ fast and simple to deploy.

> automatically handles the deployment details of capacity provisioning, auto-scaling, load balancing, and application health monitoring. Within minutes, an application will be ready to use without any infrastructure or resource configuration work on your part.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; $+$ scalable

> It automatically scales an application up and down based on application’s need using easily adjustable Auto Scaling settings.


&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  $+$ developer productivity

> focus on writing code rather than spending time managing and configuring servers, load balancers, databases, firewalls, and networks.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; $+$ complete infrastructure control

> If you decide you want to take over some (or all) of the elements of your infrastructure, you can do so seamlessly.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; $+$ the ability to select a policy to update (rolling, rolling with batch, immutable, traffic splitting deployments).

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; $-$ can be expensive, if using an immutable deployment policy, for example.


6. Using AWS Cognito as solution for users sign-in/sign-up

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; $+$ cost-effective 

> It has a free tier of 50,000 MAUs for users who sign in directly to Cognito User Pools and 50 MAUs for users federated through SAML 2.0 based identity providers.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; $+$ secure

> allows set up multi-factor authentication (MFA) with each account, uses SSL/TLS to communicate with AWS resources.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; $+$ supports login with social identity providers and SAML or OIDC-based identity providers.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; $+$ supports various compliance standards, operates on open identity standards (OAuth2.0, SAML 2.0 and OpenID Connect).

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; $-$ no disadvantages found, this service is a really flexible and allows to write less code.

![authorization](/assets/auth.jpg)

7. Using AWS Codebuild, Codedeploy, Codepipeline, Elastic Beanstalk for deploy code, CI/CD process

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; $+$ CodePipeline allows to launch CI/CD proccess automatically after merge code into master branch into GIT.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; $+$ integration with Elastic Beanstalk

$-$ 
$-$ 

![deploy scheme](/assets/deploy.jpg)

## Storage of configurations

I chose Parameter Store of AWS Systems Manager for storing secrets and environment variables, because:
- works with Lambda and EC2. 
- possible to manage all confuguration ouside code.
- secure way to provide env, secrets to the code.
- no cost, if using Standart type (10,000 parameters allowed per Region, 4KB - maximum size of a parameter value)

![configuration scheme](/assets/config.jpg)

## Communication between services through a queue

I chose AWS SQS for сommunication between services, because:
- convenient since the whole system is built on AWS services
- good integration with Lambda
- easy to set up, if compare with Apache Kafka, RabbitMQ
- using features like FIFO queue, process missing messages with Dead letter queue, set up delay
- cost-effectively (1 million Amazon SQS requests for free each month)


## Schemes
[All schemes](https://miro.com/app/board/uXjVP9_EPIk=/?openComment=3458764540430567198&utm_medium=feed&utm_source=miro).


The serverless solution scheme

![serverless scheme](/assets/serverless.jpg)

The server solution scheme

![server scheme](/assets/server.jpg)