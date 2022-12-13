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
