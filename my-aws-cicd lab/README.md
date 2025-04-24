# my-aws-cicd


## 1. Code Commit
========================


### Go to CodeCommit page 

Click on "Create repository"

```sh
codecommit-repo
```

 
### Go to IAM & Create User with Permissions

Username 👇

```sh
demo-codecommit
```

Policy 👇

```sh
AWSCodeCommitFullAccess
AWSCodeCommitPowerUser
IAMUserChangePassword
```

Open the User you created  &  Go to the Security Credentials tab

Scroll down to HTTPS Git credentials for AWS CodeCommit

Select Generate Credentials  &  Download credentials



### clone the repository  & Push the local changes to the CodeCommit repository



Create an Ubuntu-22 Terminal   ---->   Connect your Instance (terminal)

```sh
sudo apt update
```

```sh
sudo git clone https://git-codecommit.us-east-2.amazonaws.com/v1/repos/codecommit-repo
ls
```


```sh
cd codecommit-repo
```


```sh
sudo git init
```

```sh
sudo vi index.html
```
( Upload index.html code )



```sh
sudo git status
sudo git add .
sudo git commit -m "Adding index.html file"
sudo git push origin master
```

( Verify the same in the CodeCommit on the AWS Console )


## 2. Code Build
========================


### Create buildspec.yml and push into CodeCommit Repo


```sh
sudo vi buildspec.yml
```

( Upload buildspec.yml code )


```sh
sudo git status
sudo git add .
sudo git commit -m "Adding buildspec.yml file"
sudo git push origin master
```

( Verify the same in the CodeCommit on the AWS Console )


### Go to S3 and Create bucket for artifact


Bucket name =  my-codebuild-data 


### Go to the CodeBuild service 

Click the "Create build project"

```sh
testbuild
```

Source provider  =  AWS CodeCommit   ------>   Repository  =  codecommit-repo    ----->   Branch  =  master

Operating system  =  ubuntu   ----->   Role name  =   codebuild-servicerole    ----->   Buildspec  =  Use a buildspec file

Artifacts  =  my-codebuild-data    ---->  Name   =  codebuildfile   ----->  Artifacts packaging  =   .zip


Click on Create build project

Start build

( Check the status of the build which is Succeeded )




## 3. Code Deploy
========================


### Go to CodeDeploy

Click on "Create application"

```sh
CodeDeploy_App
```

Compute platform  =  EC2/On-premises


### Go to IAM & Create code-deploy-service-role with the below permissions

Go to IAM  ---->  Create Role   ---->  AWS service  =  CodeDeploy

Role name

```sh
code-deploy-service-role
```

Policy 👇

```sh
AmazonEC2FullAccess
AmazonEC2RoleforAWSCodeDeployLimited
AmazonEC2RoleforAWSCodeDeploy
AmazonS3FullAccess
```


### Create an EC2 instance on which you want to deploy the index.html file

Create an Ubuntu-22

Name  =  myserver


### Create a deployment group

Deployment group name


```sh
AWSCodeDeploy
```

Service role  =  code-deploy-service-role

Deployment type  =  In-place

Environment configuration  =  Amazon EC2 instances

Install AWS CodeDeploy Agent  =  never

Load balancer  =  disable


### Setup a CodeDeploy agent in order to deploy code on EC2


Open your "myserver" instance and Create bash file

sudo vi agent.sh

```sh
#!/bin/bash 
# This installs the CodeDeploy agent and its prerequisites on Ubuntu 22.04.  
sudo apt-get update 
sudo apt-get install ruby-full ruby-webrick wget -y 
cd /tmp 
wget https://aws-codedeploy-us-east-2.s3.us-east-2.amazonaws.com/releases/codedeploy-agent_1.3.2-1902_all.deb 
mkdir codedeploy-agent_1.3.2-1902_ubuntu22 
dpkg-deb -R codedeploy-agent_1.3.2-1902_all.deb codedeploy-agent_1.3.2-1902_ubuntu22 
sed 's/Depends:.*/Depends:ruby3.0/' -i ./codedeploy-agent_1.3.2-1902_ubuntu22/DEBIAN/control 
dpkg-deb -b codedeploy-agent_1.3.2-1902_ubuntu22/ 
sudo dpkg -i codedeploy-agent_1.3.2-1902_ubuntu22.deb 
systemctl list-units --type=service | grep codedeploy 
sudo service codedeploy-agent statusThe code agent is running.

```

```sh
sudo bash agent.sh
``` 


### Create scripts file and push into CodeCommit Repo


```sh
sudo mkdir scripts
cd scripts
```


```sh
sudo vi install_nginx.sh
```

( Upload install_nginx.sh code )


```sh
sudo vi start_nginx.sh
```

( Upload start_nginx.sh code )


cd ..


```sh
sudo git status
sudo git add .
sudo git commit -m "Adding scripts file"
sudo git push origin master
```

( Verify the same in the CodeCommit on the AWS Console )


### Create appspec.yml file and push into CodeCommit Repo


```sh
sudo vi appspec.yml
```

( Upload appspec.yml code )


```sh
sudo git status
sudo git add .
sudo git commit -m "Adding appspec.yml file"
sudo git push origin master
```

( Verify the same in the CodeCommit on the AWS Console )


sudo su


```sh
sudo apt update -y
sudo apt install nginx -y
sudo systemctl start nginx
sudo systemctl enable nginx
```


Go to CodeBuild   and  Start build



### Create deployment


Go to Deployments and click on 'Create deployment'

Revision location  =  s3://my-codebuild-data/codebuildfile

Revision file type  =  .zip

Create deployment

( EC2 doesn't have any role policy to retrieve the data from S3 to CodeDeploy )

 - Go to IAM
 - Create Role
 - AWS service  =  ec2
 - Role name   =  ec2-service-role
 - Policy 👇

```sh
AmazonEC2FullAccess
AWSCodeDeployFullAccess
AmazonS3FullAccess
```

Attach that service role to the EC2 instance

Restart the code-deploy agent

```sh
sudo service codedeploy-agent restart
```

-: COPY YOUR INSTANCE PUBLIC IP AND PASTE IT ON BROWSER TO VERIFY :-


## 4. Code PipeLine
========================


### Go to the CodePipeline console

Click "Create pipeline."

Pipeline name

```sh
AWS-CICD
```

Service role  =  New service role   ---->  Next

Source provider

 - AWS CodeCommit
 - Repository name   =  codecommit-repo
 - Branch  =  master

Next

Build provider

 - AWS CodeBuild
 - Region  =  ohio
 - Project name   =  testbuild

Next


Deploy provider
 - AWS CodeDeploy
 - Region  =  ohio
 - Application name   =  CodeDeploy_App
 - Deployment group  =   AWSCodeDeploy

Next


CREATE PIPELINE




### Now Change Code In your INDEX.HTML file 😊😊😎😎


The pipeline will automatically trigger a build and deploy the new code to the EC2 instance



=========================END=====================================================
