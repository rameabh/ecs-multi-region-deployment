## Summary:
This repository contains coludformation code for multi region deployment of ECS on fargate cluster.customers can expand application globally running application across multiple regions. Whether you need a multi-region architecture to support disaster recovery or bring your applications closer to your customers this code will walk you through how to deploy underlying resources to expand container based app into multiple regions.

## Steps to deploy
We will deploy a sample application in the two ECS clusters in two different AWS Regions, install AWS Load Balancer Controller in both the clusters, and expose the application in both regions using Application Load Balancer (ALB). Then we will configure these ALBs as endpoints in AWS Global Accelerator. We will configure one region to be the primary and other as failover.

## Pre-requisites

    1. AWS CLI installed
    2. AWS CLI is configured properly to connect to your AWS account
    3. Make sure there is a application container image to be deployed into cluster
    4. Make sure you have a hostedzone in the account you are creating the clusters.

## inputs

    # Pre-deploy
    VPC:           Name of an existing VPC for the ECS cluster.
    SubnetA:       Subnet ID of an existing Subnet.
    SubnetB:       Secoundary Subnet ID of an existing Subnet.
    PrivateCAarn: Update with the certificate ARN from Certificate Manager, which must exist in the same region.
    Image: Update with the Docker image. "You can use images in the Docker Hub registry or specify 
    ServiceName:    Service name for ECS cluster
    ContainerPort:  Port container to be run on
    LoadBalancerPort:   LB port
    HealthCheckPath: Health check for container
    HostedZoneName: DNS hosted Zone
    Subdomain:      DNS sub-domain
    MinContainers:  Min number of containers to be depoloyed
    MaxContainers:  Max number of containers to be depoloyed
    AutoScalingTargetValue: autoscaling target value to container scaling

    # deploy
    PreDeployStackName:  Stackname of the pre-deployment cluster.

    # Post deploy
    DeployStackName: Stackname of the deploy stack from previous step. 

## Run Sample instructions

    ## Make changes to Parameters in fargate-pre-deploy1.yaml

    # aws cloudformation deploy --template-file fargate-pre-deploy1.yaml --stack-name fargate-pre-deploy --region us-east-1
    # aws cloudformation deploy --template-file fargate-deploy.yaml --stack-name fargate-deploy --capabilities CAPABILITY_NAMED_IAM --region us-east-1
    # aws cloudformation deploy --template-file fargate-post-deploy1.yaml --stack-name fargate-post-deploy --region us-east-1

    ## Make changes to Parameters in fargate-pre-deploy2.yaml

    # aws cloudformation deploy --template-file fargate-pre-deploy2.yaml --stack-name fargate-pre-deploy --region us-west-2
    # aws cloudformation deploy --template-file fargate-deploy.yaml --stack-name fargate-deploy --capabilities CAPABILITY_NAMED_IAM --region us-west-2
    # aws cloudformation deploy --template-file fargate-post-deploy2.yaml --stack-name fargate-post-deploy --region us-west-2

