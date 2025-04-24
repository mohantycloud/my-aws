# vpc-peering



VPC peering is a networking connection established between two VPCs that enables you to route traffic between them using private IP addresses.

VPC peering is essential for enabling private communication between VPCs without relying on public IPs or the internet. 

It facilitates secure and efficient sharing of resources across different VPCs, whether they reside within the same account, different accounts, or even different regions.


### Task 1: Creating Two VPCs

vpc-1    ------>   ip = 10.100.0.0/16


vpc-2   --------->   ip =  10.200.0.0/16


### Task 2: Creating Internet gateway


igw-1-vpc-1   &  attach to vpc-1


igw-1-vpc-2   &  attach to vpc-2


### Task 3: Creating Subnets


subnet-1-vpc-1   ----->  vpc-1    ---->   ip = 10.100.1.0/24    ----->   az  =  ap-south-1a

subnet-1-vpc-2   ----->  vpc-2    ---->   ip = 10.200.1.0/24    ----->   az  =  ap-south-1b



### Task 4: Creating Route tables


rt-1-vpc-1   ----->   vpc-1   ---->  associate  subnet-1-vpc-1  &  mention  igw-1-vpc-1

rt-1-vpc-2   ----->   vpc-2   ---->  associate  subnet-1-vpc-2  &  mention  igw-1-vpc-2



### Task 5: Creating EC2 instances


Name  =  firstinstance    ------>    AMI   =  Ubuntu-22     ------>   KEYPAIR  =  peer-key.pem   

vpc  =  vpc-1    ---->  autoassign-pub-ip  =  enable     ----->   SG  =  vpc1-sg ( alltraffic  -->  anywhere )

userdata  :-  paste userdata code-for-server1

```
#!/bin/bash

# Update and install apache2
apt-get update
apt-get install -y apache2

# Create a simple index.html file with "Hello" text
echo "Hello FIRST SERVER" > /var/www/html/index.html

# Restart apache2 service
systemctl restart apache2
```

launch instance



Name  =  secondinstance    ------>    AMI   =  Ubuntu-22     ------>   KEYPAIR  =  peer-key.pem   

vpc  =  vpc-2    ---->  autoassign-pub-ip  =  enable      ----->   SG  =  vpc2-sg ( alltraffic  -->  anywhere )

userdata  :-  paste userdata code-for-server2

```
#!/bin/bash

# Update and install apache2
apt-get update
apt-get install -y apache2

# Create a simple index.html file with "Hello" text
echo "Hello SECOND SERVER" > /var/www/html/index.html

# Restart apache2 service
systemctl restart apache2
```

launch instance




### Task 6: Establishing a VPC Peering Connection


name  =  mypeer

requester-vpc  =  vpc-1

accepter-vpc  =   vpc-2


click Accept request 


click on  Route table ( rt-1-vpc-1  )  ---->   Edit routes   ---->   Add route  =  10.200.0.0/16 ( vpc-2  ip-add )   ---->  mention peer-id   ---->   Save changes


click on  Route table ( rt-1-vpc-2  )  ---->   Edit routes   ---->   Add route  =  10.100.0.0/16 ( vpc-1  ip-add )   ---->  mention peer-id   ---->   Save changes




### Task 7: SSH into EC2s


Select your firstinstance and click on Connect    ----->   click On SSH client   --->  open your powershell & connect

sudo apt update

curl  < pvt-ip-secondinstance >


(successfully established)





Select your secondinstance and click on Connect    ----->   click On SSH client   --->  open your powershell & connect

sudo apt update

curl  < pvt-ip-firstinstance >


(successfully established)




===========

Now, let's remove the connection and see if the connection still happens.

curl  < pvt-ip-secondinstance >

There is no response to the request at this time.





================end=============================
