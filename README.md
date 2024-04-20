# Project Overview: CI/CD with GitHub Actions

## Overview

This project implements Continuous Integration and Continuous Deployment (CI/CD) pipelines. The pipeline automates various stages including testing, building Docker images, and deploying to AWS ECS.

## Setting Up CI/CD with GitHub Actions

### GitHub Setup

1. **Generate SSH Key Pairs**: Generate SSH key pairs and upload the public key to the GitHub account.
2. **SonarQube Integration**: Create a SonarQube organization, generate a token, and set up quality gates.
3. **Secrets Configuration**: Add necessary secrets such as SonarQube information, AWS access keys, AWS account ID, ECR repository address, and RDS information in the GitHub repository secrets.

### GitHub Actions Workflow

1. **Testing Workflow**: 
   - Job: TESTING
   - Steps: 
     - Checkout the repository
     - Run Maven tests
     - Run Maven checkstyle
     - Setup Java environment
     - Setup SonarQube
     - Run SonarQube scanner
     - Check SonarQube quality gate

2. **Build and Publish Workflow**:
   - Job: BUILD_AND_PUBLISH
   - Steps:
     - Checkout the repository
     - Modify database information in code using sed
     - Build and upload Docker image to Amazon ECR

3. **Deployment Workflow**:
   - Job: DEPLOY
   - Steps:
     - Deploy on Amazon ECS (Refer to marketplace for suitable deployment options)

### AWS Setup

1. **IAM User Creation**: 
   - Create an IAM user and attach policies `AmazonEC2ContainerRegistryFullAccess` and `AmazonEC2ContainerServiceFullAccess`. 
   - Generate access keys for the user.

2. **ECR Repository Creation**: Create an ECR repository to store Docker images.

3. **RDS Setup**: 
   - Create an RDS instance. 
   - Create an EC2 instance to run initial queries on the RDS.

### Continuous Deployment on AWS

1. **ECS Cluster Creation**: 
   - Create an ECS cluster.
   - Create a task definition (selecting our image from ECR).
   - Edit the execution role of the task definition to attach the `CloudWatchLogsFullAccess` policy.

2. **Service Configuration**: 
   - Create a service and allow port 3306 from the security group of the service to the security group of RDS.
   - Copy the JSON of the task definition, create a `taskdeffile.json` in our code, and paste the JSON into it.

3. **Continuous Deployment Process**:
   - Push code changes to the repository.
   - GitHub Actions workflow will automatically trigger testing, building, uploading to ECR, and deploying on ECS.

