# auto-scaling-lab ( Mumbai - Region )

#### 1. Create launch template

Launch template name = my-lt

Template version description = 1

Launch template contents ------>  AMI = windows  ----> Instance type =  t2.micro  ---> Key pair (login) = create keypair ( asl.pem )

create launch template


#### 2. Create Auto Scaling group

Auto Scaling group name = my-asg

Launch template = my-lt ( mention your LT )   ---->  click on next

VPC = Default/custom   ---->  Availability Zones and subnets = ap-south-1a & ap-south-1b

click on next

Load balancing  = No load balancer

Health check grace period = 300second   ---->  next

Min desired capacity = 1    ,  Max desired capacity  = your choice

Automatic scaling policy = Target tracking scaling policy    ----->  Target value = 50%  

click on next

Add notifications  ---->  create topic ----> Send a notification to ( topic name ) = my-notification

With these recipients  = Mention your mail-id ( Go to your mail-id & confirm subscription )

next

create auto scaling group

#### 3. Login Your Instance and generate load

Go inside server

create a text file and inside file type " a.bat "    ------>  save as " a.bat "  , all files

click multiple times on " a.bat file "

#### 3. Go and Check
