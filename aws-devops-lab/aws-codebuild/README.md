# AWS-CodeBuild

### Step 1 - Beside your repo, click on Build  --->  Build Projects  --->  create build project👩🏾‍🔧

### Step 2 - Project name

```sh
my-app-build
```

### Step 3 - Source provider and Repository , Branch

```sh
AWS-CodeCommit
my-app
master
```

### Step 4 -  Operating System

```sh
Ubuntu
```

### Step 5 -  Service Role  --->  New Service Role

```sh
codebuild-m-service-role
```

### Step 6 -  Buildspec  --->  Use a buildspec file

```sh
buildspec.yml
```

In buildspec, you need to create a YAML file 

```sh
version: 0.2

phases:
  install:
    commands:
      - echo Installing NGINX
      - sudo apt-get update
      - sudo apt-get install nginx -y
  build:
    commands:
      - echo Build started on `date`
      - cp index.html /var/www/html/
  post_build:
    commands:
      - echo Configuring NGINX

artifacts:
  files:
    - '**/*'
```

### Step 7 - Now We need to Push our buildspec.yml into CodeCommit Repository

```sh
git status
git add .
git commit -m "my buildspec file"
git push origin master
``` 

Go to your repo, we can see our buildspec.yml file is also available here.


### Step 8 - Artifacts  --->  Create an S3 Bucket

```sh
my-app-bucket-24

Name = mybuild

Artifacts packaging = .zip
```
Untik CloudWatch Logs

### Step 9 - Click on start build.


Go to your bucket, we can see our build file is also available here.
