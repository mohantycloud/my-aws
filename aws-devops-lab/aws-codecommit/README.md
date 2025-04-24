# AWS-CodeCommit

### Step 1 - Login to your AWS console and search for codecommit.

### Step 2 - Now create a repository, by giving some name & description.

```sh
my-app
```

```sh
This is my codecommit repository
```
( ⚠️You are signed in using a root account. You cannot configure SSH connections for a root account, and HTTPS connections for a root account are not recommended. Consider signing in as an IAM user and then setting up your connection.⚠️ )

### Step 3 - So we need to create a user and IAM users permissions

```sh
devbabu
```

```sh
AWSCodeCommitPowerUser
```

### Step 4 - Now click on your user --> security credentials, look for HTTPS GIT CRE.... and click Generate Credentials.

### Step 5 - Now click Download Credentials.

### Step 6 - Launch an Linux-Instance For Terminal.

### Step 7 - Now we need to clone our My-app repo locally.

```sh
sudo su -
mkdir aws
cd aws/
yum install git -y
```
git clone < CodeCommit Repo Url >

```sh
cd my-app/
vi index.html
```

### Step 8 - Now We need to Push our Index.html into CodeCommit Repository

```sh
git status
git add .
git commit -m "my html file"
git push origin master
```
