# Launch Windows-ec2-instance & Deploy website 👨‍💻


#### 1. Create Windows instance

#### 2. Instance Type = t2.micro

#### 3. Create a new keypair = mywinkey.pem

#### 4. VPC = default or custom

Subnet  = ap-south-1A  or No Preference

Auto-assign public IP  =  Enable

Firewall (security groups)  =  Create a new security group   or You can use existing security group

- rdp   =  anywhere
- http  =  anywhere
- https =  anywhere
  

#### 5. Number of instances = 1

Launch Instance


#### 6. Connect your instance :-

Select your Instance and click on connect

Download RDP and Upload key file

Decrypt password and copy password

Open RDP and paste password ( ctrl+v )

Allow YES

Go to Server manager  ---->  Enable IIS-webserver and .net framework   ---->  Click install

Go to File explorer  --->  this pc  --->  inetpub  --->  wwwroot  --->  delete existing files

upload or create your index.html file

#### 7. Copy Instance Public-ip & paste it on Browser 🌏⛳🚀✌️


#### 8. TERMINATE ALL YOUR RESOURCES


========================== END========================================
