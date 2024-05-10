# AWS CI/CD Pipeline for Containerized Web Application Deployment
## Overview:

This project demonstrates the implementation of a CI/CD pipeline to automate the deployment process of a containerized web application on AWS. The pipeline leverages various AWS services including CodePipeline, CodeBuild, ECS, and CloudFormation for infrastructure provisioning.
## Components:

- Simple Website: 
The web application consists of an index.js file and a package.json file. These files contain the basic structure and functionality of the web application.
- Dockerfile:
This file contains instructions to build the Docker image for the web application. It specifies the base image, dependencies, and commands needed to run the application within a Docker container.
- buildspec.yml:
Defines the build instructions for CodeBuild to build the Docker image and push it to Amazon ECR. This file includes commands to install dependencies, build the application, and push the Docker image to the ECR repository.
- CloudFormation Templates: With core_infrastructure and ecs-ec2-with-cf.yml under cloud_formation_stacks folder ,  CloudFormation templates are provided to create necessary AWS resources like VPC, ECS cluster, auto-scaling groups, security groups, etc. These templates define the infrastructure required to host and run the web application.
- Task Definition:
A new task definition is created(uing aws console) to run the web application container using the image from Amazon ECR. This task definition    specifies the Docker image URI, container port, CPU/memory requirements, and other container settings.
- CodePipeline Configuration:
A CodePipeline is configured with source set to the GitHub repository, build stage with CodeBuild, and deploy stage with ECS. The pipeline automates the entire CI/CD process, from source code changes to deployment on ECS.

## Usage:

To deploy the web application from scratch, follow these steps:

- Setup Infrastructure:
        Navigate to the cloud_formation_stacks folder.
        Deploy the CloudFormation templates using  AWS CLI to create the required infrastructure on AWS. This includes VPC, ECS cluster, auto-scaling groups, security         groups, etc :

      aws cloudformation create-stack --stack-name ecs-type-ec2 --capabilities CAPABILITY_IAM --template-body file://./ecs-ec2-with-cf.yml
      aws cloudformation create-stack --capabilities CAPABILITY_IAM --stack-name ecs-core-infrastructure --template-body file://./core-infrastructure-setup.yml
    
- Build Docker Image :
        Ensure you have set up the necessary AWS credentials and permissions to access CodeBuild and ECR(configure codebuildservice role as metionned in the
        codeBuildServiceRole.md file)
        CodeBuild will build the Docker image using the provided Dockerfile and push it to Amazon ECR.

- Deploy to ECS:
        Once the Docker image is pushed to ECR, create a task definition to run the web application container using the image from Amazon ECR .
        Then create a service with this task definition on your cluster .
  
  ![image](https://github.com/firassaada/AWS-CI-CD-Pipeline-for-Containerized-Web-Application-Deployment/assets/94303698/b36f825c-80f1-4156-86b3-89c0b3b845f5)

  ![image](https://github.com/firassaada/AWS-CI-CD-Pipeline-for-Containerized-Web-Application-Deployment/assets/94303698/6eaf108d-8d2f-45e3-a619-ad683e3d8bd3)

  ![image](https://github.com/firassaada/AWS-CI-CD-Pipeline-for-Containerized-Web-Application-Deployment/assets/94303698/06dbc742-eec2-4799-8270-40b9307f4b91)

- Create CodePipeline:

    Navigate to the AWS CodePipeline console.
    Click on "Create pipeline" and provide a name for your pipeline.
    Choose "GitHub" as the source provider and authenticate with your GitHub account.
    Select the repository and branch you want to trigger the pipeline.
    Click on "Next step".

- Configure Build Stage:

    For the build stage, choose "AWS CodeBuild" as the build provider.
    Select the CodeBuild project you created earlier.
    Click on "Next step".

- Configure Deploy Stage:

    For the deploy stage, choose "Amazon ECS" as the deploy provider.
    Select the ECS cluster where you want to deploy your application.
    Choose the task definition and service within the ECS cluster.
    Specify the image repository and tag to use for deployment(froom ECR).
    As part of the ECS deployment stage in the pipeline, provide the imagedefinitions.json artifact to the ECS deployment task.
    Click on "Next step".

Review and Create Pipeline:

  Review the pipeline configuration to ensure everything is set up correctly.
  Click on "Create pipeline" to create the pipeline.
  The pipeline will now be created, and the initial run will be triggered automatically if you have any commits in the selected branch.

![image](https://github.com/firassaada/AWS-CI-CD-Pipeline-for-Containerized-Web-Application-Deployment/assets/94303698/51d57e03-e7a2-4997-bd34-1cd9b8090d0d)

## Contributing:

Contributions to enhance the CI/CD pipeline or improve the infrastructure setup are welcome. Fork the repository, make your changes, and submit a pull request.
