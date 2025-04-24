## AWS Devops CICD Project


## AWS-CodeCommit

##### Step 1 - Now create a repository, by giving some name & description.

```sh
CICD-project
```

```sh
This is my codecommit repository
```

##### Step 2 -  Create an IAM users with permissions

```sh
devbabu
```

```sh
AWSCodeCommitPowerUser
```

##### Step 3 - Now click on your user --> security credentials, look for HTTPS GIT CRE.... and click Generate Credentials.

##### Step 4 - Now click Download Credentials.

##### Step 5 - Create  MY-PROJECT file in desktop

##### Step 6 - Clone it now ( open code studio and open terminal )

git clone < CodeCommit Repo Url >

```sh
cd CICD-project
```

```sh
git init
```

( click CICD-project  --->  under CICD-project = create an index.html file )


```sh
git status
git add .
git commit -m "my index file"
git push origin master
```


## Create S3 Bucket

Bucket name  = my-bucket-123-dev

Folder name =  my-folder


## AWS-CodeBuild

##### Step 1 - Build Projects  --->  create build project👩🏾‍🔧

##### Step 2 - Project name

```sh
my-build-project
```

##### Step 3 - Source provider and Repository , Branch


AWS-CodeCommit  --->   my-app   ---->   master


##### Step 4 -  Operating System

Ubuntu

##### Step 5 -  Service Role  --->  New Service Role

```sh
codebuild-m-service-role
```

##### Step 6 -  Buildspec  --->  Use a buildspec file

```sh
buildspec.yml
```

In buildspec, you need to create a YAML file 

( click CICD-project  --->  under CICD-project = create a buildspec.yml file )


```sh
git status
git add .
git commit -m "my buildspec file"
git push origin master
```


Go to your repo, we can see our buildspec.yml file is also available here.


##### Step 8 - Artifacts  --->  Choose S3 Bucket


my-app-bucket-24

Name = my-folder

path = artifact.zip

Artifacts packaging = .zip

Untik CloudWatch Logs

##### Step 9 - Click on start build.


Go to your bucket, we can see our build file is also available here.



## AWS-CodeDeploy


##### Step 1 - Click Create application.

```sh
my-app-project
```

Compute platform   --->  EC2/on-premises

##### Step 2 - Create CodeDeploy IAM role

Create Role   --->  AWS Service = codedeploy

Role Name
```sh
mycode-deploy-role
```

Attach Policy to role

```sh
AmazonEC2FullAccess
AmazonEC2RoleforAWSCodeDeploy
AmazonEC2RoleforAWSCodeDeployLimited
AmazonS3FullAccess
AWSCodeDeployFullAccess
```

##### Step 3 - Search for EC2 and launch an instance.

Go to EC2 & Create ubuntu-22 instance

Instance Name

```sh
my-app-ec2
```

##### Step 4 - Now click on Create Deployment group.

Deployment group name

```sh
my-app-deployment-group
```

Deployment type = In-place

Environment configuration =  Amazon EC2 Instances

Install AWS CodeDeploy Agent  =  Never

Load balancer  =  Disable

Click Create Deployment Group

##### Step 5 - Setting up the agent for CodeDeploy.

Now Connect your EC2 instance and run the following code

```sh
vim install.sh
```

Paste the below code

```sh
#!/bin/bash
# This installs the CodeDeploy agent and its prerequisites on Ubuntu 22.04. 
sudo apt-get update
sudo apt-get install ruby-full ruby-webrick wget -y
cd /tmp
wget https://aws-codedeploy-us-east-1.s3.us-east-1.amazonaws.com/releases/codedeploy-agent_1.3.2-1902_all.deb
mkdir codedeploy-agent_1.3.2-1902_ubuntu22
dpkg-deb -R codedeploy-agent_1.3.2-1902_all.deb codedeploy-agent_1.3.2-1902_ubuntu22
sed 's/Depends:.*/Depends:ruby3.0/' -i ./codedeploy-agent_1.3.2-1902_ubuntu22/DEBIAN/control
dpkg-deb -b codedeploy-agent_1.3.2-1902_ubuntu22/
sudo dpkg -i codedeploy-agent_1.3.2-1902_ubuntu22.deb
systemctl list-units --type=service | grep codedeploy
sudo service codedeploy-agent status

```

Run this CodeDeploy agent on ec2 instance.

```sh
bash install.sh
```

##### Step 6 - Now We need to Push our appspec.yml and scripts File into CodeCommit Repository

( click CICD-project  --->  under CICD-project = create an appspec.yml file )


```sh
git status
git add .
git commit -m "my appspec file"
git push origin master
```

( click CICD-project  --->  under CICD-project = create an scripts folder  --> create install_nginx.sh  and  start_nginx.sh )


```sh
git status
git add .
git commit -m "my scripts file"
git push origin master
```


Now Refresh your Repository, and look for the newly added files.

#### Again build the app so that the updated source code of the application can be reached to artifact(S3).

Go to CodeBuild  --->  Start Build


##### Step 8 - Create deployment

Revision location = s3://my-app-bucket-24/mybuild

Revision file type = .zip

Create Deployement

##### Step 9 - Create IAM Role creation for instances

Create Role   --->  AWS Service = EC2

Role Name
```sh
myec2-cicd-role
```

Attach Policy to role

```sh
AmazonEC2FullAccess
AmazonS3FullAccess
AWSCodeDeployFullAccess
```

Goto your running instance  -->  Actions  -->  Security  --> Modify IAM Role


##### Step 10 - Now connect and restart your instance, by using the below command.

```sh
sudo service codedeploy-agent restart
```


###### Now Copy your Public_IP and check it in the browser
