# Re-Architecting an Application on the AWS Cloud

This application was inspired by devopshydclub repository found [here](https://github.com/devopshydclub/vprofile-project)

You can see the most used languages in this project below:

![Your Repository's Stats](https://github-readme-stats.vercel.app/api/top-langs/?username=fahimtq1&theme=blue-green)

## Project brief

The scenario is of a DevOps engineer who has been given a multi-tier application stack, that has been deployed on the cloud, and has been tasked with re-architecting the application framework ready for redeployment on the cloud. Currently, the project services are running on a combination of physical, virtual and cloud machines; multiple teams are also required. Consequently, the teams are struggling with application uptime and scaling, as well as dealing with the manual processes. 

Fortunately, the cloud offers a range of benefits that solve this problem. The solution is to utilise SaaS and PaaS services, instead of the previous IaaS services. Thus, this would lead to lower operational overhead, the exchange of capital expenses with operational expenses, easier infrastructure management and automation. 

The main goal of this task is to implement the "re-architect" cloud migration strategy. Re-architecting is the process of breaking an application down and designing it's framework, so that it takes advantage of cloud-native features. This is in contrast to the [Lift-and-Shift](https://github.com/fahimtq1/vprofile-cloud-project/blob/main/README.md) strategy, as it is more time-consuming and requires a deeper knowledge of cloud services.

## Architectures

### Application architecture

![vprofile-project-architecture](https://user-images.githubusercontent.com/99980305/199768482-3bb654c1-8a40-4352-8e86-49b4ae50875f.png)

#### Application architecture explained

These are the basic steps of the application workflow:

- Users send requests that are received by the load-balancing service
- The application load balancer is a load-balancing service that receives that allows the frontend to listen on port 80 and then routes the request to the application (Tomcat) server on port 8080
- The Tomcat server receives the request on port 8080 and communicates with the backend services, on their respective ports, to receive the application content, which can then be routed to the frontend so the users can view it

### Original cloud architecture

This mimics the application architecture, except the services are replaced with the AWS services, with the EC2 instances acting as both the backend and frontend servers

![vprofile-cloud-architecture](https://user-images.githubusercontent.com/99980305/202738252-d0a9176b-7e8f-42fe-a734-449985724cbc.png)

#### Services

- EC2 instances
- Application Load Balancer
- Autoscaling Group
- Target Group
- Launch configuration
- AMI
- S3 bucket

### Re-architected cloud architecture

![vprofile-rearchitect](https://user-images.githubusercontent.com/99980305/207237474-5e194f2c-a570-4810-bd62-659454b1cc9d.png)

#### Services

- Elastic Beanstalk
- S3 bucket
- RDS
- Elasticache 
- Amazon MQ
- Route 53
- CloudFront
- CloudWatch (optional monitoring capabilities)

#### Re-architected cloud architecture explained

- Users make HTTP requests to a domain name (application webpage)
- Route 53 (DNS) directs these requests to the relevant IP addresses 
- CloudFront is a Content Delivery Network (CDN), which speeds up the requests journey to reduce latency
- The requests are routed to the Elastic Beanstalk (PaaS) container, which handles the deployment and management of the application
- Elastic Beanstalk deals with the load balancing, autoscaling, and health-monitoring of the application
- The EC2 instances, used by Elastic Beanstalk, will retrieve data from an S3 bucket that has all the application files stored within it
- The backend services will use the following: RDS, ElastiCache (for Memcached) and Amazon MQ
- These backend services will communicate with the EC2 instances to provide the content to the end-users

## Cloud setup

### Plan

The basic flow of execution is as follows:

- Login to AWS
- Create a key pair for secure access to the Elastic Beanstalk instance
- Create security groups for ElastiCache, RDS and Amazon MQ
- Create the ELastic Beanstalk environment
- Update the security group of the backend services to allows traffic from the Elastic Beanstalk security group
- Update the security group of the backend services to allow internal traffic between the backend services
- Launch an EC2 instance to initialise the RDS database
- Build the application source code artefact locally and deploy to Elastic Beanstalk
- Create a CDN with an SSL certificate
- Update the CNAME record on chosen webhost (using GoDaddy)
- Test the URL

### Steps

#### Security groups and key pairs

- Navigate to the EC2 console and create a key pair, which will be used to access the Elastic Beanstalk instance. 
- Create a security group with a dummy rule, so later on this rule can allow internal traffic access between the backend services that use the same security group
    - This can only be done once the security group has been made

#### RDS

- Navigate to the RDS console

- Go to Subnet groups on the left menu
- Select Create DB subnet group
- Provide an appropriate name
- Select the default VPC
- Select all the Availability Zones and all the Subnets- this allows resiliency
- Create the subnet group

- Go to Parameter groups in the left menu
- Select Create parameter group
- Select `aurora-mysql5.7` as the parameter group family
- Provide an appropriate name
- Create the parameter group

- Go to Databases in the left menu
- Select Create database
- For the database creation method, select Standard create
- Engine options:

    - Engine type- MySQL
    - Edition- Amazon Aurora MySQL-Compatible Edition
    - Version- 5.7.22

- Template- Dev/Test
- Provide an apporpriate name
- Master username- admin
- Select Auto generate password
- Instance configuration:

    - DB instance class- Burstable classes t3.micro

- Storage configurations remain default, but ensure the Enable storage autoscaling option is enabled
- Select the relevant DB subnet group (created earlier)
- Select No public access
- Select the relevant security group (created earlier)
- Select enable enhanced monitoring under Monitoring, with 60 seconds of granularity
- In Additional configuration, select the relevant parameter group (created earlier)
- Create the database
- Once the instance has been made, the credentials should be saved locally (perhaps a `.txt` file)

#### ElastiCache

#### Amazon MQ

#### Database initialisation 

#### Elastic Beanstalk

#### Update security groups

#### Build and deploy the artefact

#### CloudFront
