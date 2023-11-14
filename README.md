## Belong Platform Engineering Coding Challenge Solution ##
### Overview ###
This repository contains AWS CloudFormation templates that provide a solution to the Belong Platform Engineering Coding Challenge. The challenge involves creating a standard, secure AWS infrastructure with specific requirements.

### Infrastructure Components ###
- ![Infrastructure](belong.png)

The solution consists of two main components:
1. Network Infrastructure (network.yaml)
This template sets up the foundational network infrastructure within AWS:

- VPC: A dedicated Virtual Private Cloud (VPC) with CIDR block 10.0.0.0/16.
- Subnets:
    Public Subnets: Two subnets (10.0.0.0/18 and 10.0.64.0/18) with internet access.
    Private Subnets: Two subnets (10.0.128.0/18 and 10.0.64.192/18) without direct internet access.
- Internet Gateway: For outbound internet access from the public subnets.
- NAT Gateway: To enable outbound internet access for instances in the private subnets.
- Gateway Endpoint: for ec2 to access s3.
- Interface Endpoint: for ec2 instance to access cloudwatch logs.

2. Application Infrastructure (app.yaml)
This template creates the resources required to host and manage the application:

- EC2 Launch Template: Configures EC2 instances in the private subnet, running httpd service. The time zone of these instances is set to AEST (Australian Eastern Standard Time).
- Auto Scaling Group: Maintains the desired count of EC2 instances and automates their creation in the private subnets.
- S3 Bucket Access: Instances are configured to download belong-test.html from the belong-coding-challenge bucket in the Sydney region.
- Bastion Host: An EC2 instance in the public subnet for secure SSH access to instances in the private subnet.
- Launch Template:
  Defines the configuration for EC2 instances.
  Includes settings like the AMI ID, instance type, and user data script for initializing httpd and setting the time zone to AEST.Configures downloading of belong-test.html from an S3 bucket and serving it via httpd.
- Auto Scaling Group (ASG):
  Manages the scaling and health of EC2 instances in the private subnet.
  Works in conjunction with the Application Load Balancer for traffic distribution and health checks.
- Application Load Balancer (ALB): Routes traffic to the EC2 instances and performs health checks.
### TODO ###
- ADD CloudTrail, aws inspector and guardduty