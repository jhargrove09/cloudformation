# AWS 3-Tier Architecture — CloudFormation

This repository contains a CloudFormation template that automates the deployment of a 3-tier AWS architecture, including a custom VPC, three public subnets across multiple Availability Zones, an Application Load Balancer, and an Auto Scaling Group of EC2 instances running Apache with a CPU-based scaling policy.

This template recreates the same architecture originally built manually through the AWS Console, now fully defined as code and deployable with a single stack.

## Architecture

- Custom VPC with three public subnets across separate Availability Zones
- Internet Gateway and public route table
- Two-tier security group pattern — the ALB security group accepts traffic from the internet, while the web server security group only accepts traffic from the ALB, blocking any direct access to EC2 instances by public IP
- Launch Template with user data that installs Apache and serves a custom web page, with automatic detection of `dnf` vs `yum` depending on the AMI
- Application Load Balancer with a target group and listener
- Auto Scaling Group (min 2, max 5) registered to the target group
- Target tracking scaling policy maintaining 50% average CPU utilization

## Deployment

The template was deployed by uploading it to an S3 bucket and referencing the resulting S3 URL as the CloudFormation template source, rather than uploading the file directly. This mirrors how templates are typically managed in a real environment.

To deploy:

1. Upload `cloudformation.json` to an S3 bucket
2. In the CloudFormation console, choose **Create Stack** and provide the S3 URL
3. Name the stack and deploy — no parameters are required
4. Once the stack reaches `CREATE_COMPLETE`, retrieve the Load Balancer DNS name from the **Outputs** tab and use it to verify the deployment in a browser

## Read the full write-up

A detailed walkthrough of this build, including the troubleshooting process and the manual architecture this template was based on, is available here:

- [How To: Automate a 3-Tier AWS Architecture with CloudFormation](https://medium.com/@jacqueline.hargrove)
- [How To: Deploy a 3-Tier AWS Architecture with Auto Scaling and EFS](https://medium.com/@jacqueline.hargrove/how-to-deploy-a-3-tier-aws-architecture-with-auto-scaling-and-efs-4a523e2de0c2)
