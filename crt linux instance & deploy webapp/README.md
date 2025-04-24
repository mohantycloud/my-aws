# Launch linux-ec2-instance & Deploy website 👨‍💻


#### 1. Create Linux-2 instance

#### 2. Instance Type = t2.micro

#### 3. Create a new keypair = mylinkey.ppk

#### 4. VPC = default or custom

Subnet  = ap-south-1A  or No Preference

Auto-assign public IP  =  Enable

Firewall (security groups)  =  Create a new security group   or You can use existing security group

- ssh   =  anywhere
- http  =  anywhere
- https =  anywhere
  

#### 5. Number of instances = 1

Launch Instance


#### 6. Connect your instance :-

Copy Instance Public IPv4 address  and paste it on host name section ( putty )

Click on SSH ( putty )  ---->  Again Click on AUTH  ( putty )   ---->  Now Click on Credentials   ---->  private key file authentication = my-linux-key.ppk

Click on Open  --->  Click on Accept

#### 7. Login As = ec2-user

sudo su

yum update

yum install httpd -y

sudo amazon-linux-extras install php8.0 -y

cd /var/www/html

vi index.html   , Press-i

Copy an paste code inside index.html

( How to save :-  press esc  --->  :wq )

service httpd start

#### 8. Copy Instance Public-ip & paste it on Browser 🌏⛳🚀✌️


#### 9. TERMINATE ALL YOUR RESOURCES


========================== END========================================
