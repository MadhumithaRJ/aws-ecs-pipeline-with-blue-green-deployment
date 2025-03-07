<p align="center">
  <img width="460" height="300" src="https://miro.medium.com/max/700/1*kQS5gXiXi0XGjm524hNqPw.png">
</p>

<h1 align="center"><a href="https://aws.plainenglish.io/aws-codepipeline-for-amazon-ecs-part-2-a-blue-green-deployment-type-c162fd73be91">AWS CodePipeline for Amazon ECS, Part 2: A Blue/Green Deployment Type</a></h1>


 


Step 1: Create an EC2 Instance and Install Git, Docker, and AWS CLI
Launch an EC2 instance.
Install Git:
sudo yum install git -y

Install Docker:
sudo yum install docker -y
sudo service docker start

Install AWS CLI:
sudo yum install aws-cli -y
------------------------------------------------------------------------------------
Step 2: a)Create Access and Secret Keys for the IAM User and Configure on the Instance
Create Access and Secret Keys for your IAM user.

Configure AWS CLI with the generated keys:

aws configure
Enter your AWS Access Key ID.
Enter your AWS Secret Access Key.
Choose your default region (e.g., us-east-1).
Set the default output format (e.g., json).


b) Create IAM Role for CodeDeploy, CodeBuild, and ECS:
Attach the required policies like AmazonEC2RoleforAWSCodeDeploy, AmazonS3ReadOnlyAccess, AWSCodeDeployRole, AWSCodeBuildAdminAccess, etc.
-------------------------------------------------------------
Step 3: Create an Application Load Balancer (ALB) with Two Target Groups
Create an Application Load Balancer (ALB) in the AWS Management Console.
Create two target groups under the ALB:
One for the Blue environment.
One for the Green environment.
---------------------------------------------------------------
Step 4: Create ECS Cluster, Task Definition, and Service
Create an ECS Cluster in the AWS console.
Define the Task Definition (this defines the container settings and the Docker image).
Create an ECS Service that will use the previously defined task definition.
----------------------------------------------------------------
Step 5: Create ECR, Build the Image, and Push to ECR
-----------------------------------------------------------------
Step 6: Update AppSpec.yml, BuildSpec.yml, and TaskDef.json
AppSpec.yml: Configure the file to specify the deployment method and task definition.
Ensure the file points to your ECS service and task definition.

BuildSpec.yml: Define the build and deployment steps for CodeBuild.
Include commands to build the Docker image and push it to ECR.

TaskDef.json: Ensure your task definition includes the correct image URI from ECR, port mappings, and other necessary configurations.
--------------------------------------------------------------------
Step 7: Create CodePipeline and Deploy the Project
Set up a CodePipeline with stages:

Source Stage: Pull the code from your repository (e.g., GitHub, CodeCommit).
Build Stage: Use CodeBuild to build the Docker image and push it to ECR.
Deploy Stage: Use CodeDeploy to deploy to ECS with the new image.
Start the deployment through CodePipeline. It will automatically update your ECS service and deploy the new Docker image to your cluster using CodeDeploy.
-------------------------------------------------------------------
***** For more details, refer to the video: https://www.youtube.com/watch?v=3_yFkWky6hM&t=0s   ************


