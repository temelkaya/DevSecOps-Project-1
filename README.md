# DevSecOps-Project-1
Created VPC

Select IPv4 CIDR manual input

Write IPv4 CIDR # 192.158.0.0/24

No IPv6 CIDER block Created 4 subnets

##Subnet public1

###Availability Zone 1a

IPv4 CIDER – 192.158.0.0/26

Subnet public2

Availability Zone 1a

IPv4 CIDER –192.158.0.64/26

Subnet private1

Availability Zone 1a

IPv4 CIDER – 192.158.0.128/26

Subnet private2

Availability Zone 1a

IPv4 CIDER – 192.158.0.192/26 Created and attached Internet GATEWAY TO VPC Created the public and private Route Tables and attached it to VPC

Added routes to them

Go to subnet association select the top edit

In it I associated public 1 and 2 subnets with the Route tables

And I associated private 1 and 2 subnets with the Route tables Next in Subnet Public1 I created a NACL for Bastion Hosts is also called BOX or Jump Box or Jump Start (Bastion Hosts is basically an EC2. Bastion Host is used for private connectivity from public to private subnet) Elastic Cloud Compute I explicitly defined inbound rules for NACL public1

So, I gave a rule number 110

Type should be Custom TCP

Port range I gave 22 for SSH

In Source section I used my local public IP (to get my local IP I went to www.whatismyip.com or I can get it from my terminal. Just by typing ip config or you can get it from your vpc)

So I put my IP address and at the end of it I put /32 which means only single IP I explicitly defined outbound rules for NACL public1

So, I gave a rule number 110

Type should be Custom TCP

Port range I gave 1024-65535

In Source section I used my local public IP (to get my local IP I went to www.whatos,myip.com or I can get it from my terminal. Just by typing ip config or you can get it from your vpc)
Then I tested

So I put my IP address and at the end of it I put /32 which means only single IP(NOTE: in the job environment I will be using the work IP)

Then I created a Security Group for bastion in public1 Subnet

I named it and then selected my VPC I explicitly defined inbound rules for Security Group public1
Type I selected Custom TCP
Port range typed 22 for SSH
From Source dropdown menu selected My IP and my IP address pumped-up I DIDN’T explicitly define outbound rules for Security Group because it is Stateful
Next I created an EC2 instance and name it BastionHost.

I created an Amazon Machine Image

Then Instant type

I created a keypair, selected key pair type RSA and private keypair format .pem

The key pair information downloads into your download folder

Then I go to edit Network settings. Select my VPC and then select Subnet public1

On Firewall I select existing Security Group

Enable Auto-assign public IP

Common security groups section, from the dropdown menu I select bastion security group

And I launch my EC2 Next I created 1 NACL for my 2 private subnets I explicitly created an inbound rules for NACL private1 and 2

I give it a rule number 110

Port range I give 22 (SSH) (So I will be able to connect Bastion in public Subnet 1 to private Subnets)

110 SSH (22) TCP (6) 22 192.158.0.0/24 Allow

120 All ICMP - IPv4 ICMP (1) All 0.0.0.0/0 Allow

130 Custom TCP TCP (6) 1024 - 65535 0.0.0.0/0 Allow

All traffic All All 0.0.0.0/0
I explicitly created outbound for NACL private1 and 2 Rule number Type Protocol Port range Destination Allow/Deny

          110	HTTPS (443)	TCP (6)	443	0.0.0.0/0	 Allow
120 All ICMP - IPv4 ICMP (1) All 0.0.0.0/0 Allow

All traffic All All 0.0.0.0/0 Deny
Next, I go to Subnet associations and associated private 1 and 2 subnets with Network ACL

Next, I created new security groups for EC2 instances in my private subnet 1 and 2

So, I select a name for my SG
Same name I Pass into Description section
I select my own VPC
And I create an inbound rule for Security Groups for private1 and 2 EC2’s –

I select type
Custom TCP,
Port range 22,
CIDR blocks 192.158.0.0/24
And I create an outbound rule for Security Groups for private1 and 2 EC2’s –

I create 3 types – All traffic, HTTPS and All ICMP – Ipv4
IN CIDR block I select 0.0.0.0/0
Then save it Then I created 2 EC2 instances for my 2 private Subnets
I named my new EC2 instance
I selected Amazon Machine Image
I selected Amazon Linux 2 Kernel
I selected Instance Type -t2.micro
Key pair -bastion, which I have had created earlier.
I edited the Network settings
Network settings - added my VPC
Network settings – selected private 1 subnet
In Auto-assign public IP drop-down window is disabled (because don’t need an internet connectivity in private environment)
Firewall (security groups) – selected existing security group
Common security groups – selected pirvate_ec2 same as the name of the new EC2 instance
Click save Next I created the 3rd NACL for public2 subnet. I gave it an inbound and outbound rules
So, rule numbe 110
Type HTTPS
Port range 433
Source 192.158.0.0/24
Allow Then I give it one more rule
Rule number 120
Port range 1024-65535
Source 0.0.0.0/0
Allow Then I give it another one rule
Rule number 130
Type All ICMP-IPv4
Port range All
Source 0.0.0.0/0
Allow Then I configured the outbound rules too
Rule number 110
Type HTTPS
Port range 443
Source 0.0.0.0/0
Allow Then I give it one more rule
Rule number 120
Port range 1024-65535
Source 192.158.0.0/24
Allow Then I give it another one rule
Rule number 130
Type All ICMP-IPv4
Port range All
Source 0.0.0.0/0
Allow
Save changes Then I go to NAT gateways and create a NAT gateway in public2 subnet. Remember NAT Gateway still does use internet. It still traverse through internet Then I go to Subnet Association and edit it to associate NACL with public2 subnet Next, I go to edit private rout table to
So, I go to Rout table
Select private from rout table
Edit routs
Add route and I select 0.0.0.0/0 as a destination
And I add Target – Nat Gateway
and nat-02d990c73aeb94250 pops-up in the target window Next you go to Visual Studio Code and test connectivity to the internet
type ping google.com in VSC
the result should show 0% packet loss – meaning you have the connectivity to the internet Second Homework I created an s3 bucket for saving the recorded data from VPC Flow Logs Then I created a flow log
Then I copied ARN from s3 bucket’s properties section.
And paste it to S3 bucket ARN window in flow log.
Log record format I selected AWS default format
In Log file format I selected Text(default)
In Hive-compatible S3 prefix – I did not Enable
Partition logs by time I selected every 1 hour for demo purpose REMEMBER: Whenever you configure your flow log, it automatically attaches a bucket policy in your s3 bucket. Next, I configured the CloudWatch. And connected VPC Flow logs to CloudWatch
Under CloudWatch, in logs section I created a log groups
I select never expire for demo purpose Then I went to my VPC and in it I created another flowlog. This time not to s3 but to CloudWatch
And in it I select the exact same things that I had previously selected in flowlog for s3
I have to go back and create an IAM role
And then come back to Create a flow log section and select the IAM that I just created Next, I created a CloudWatch Alarm in CloudWatch. And then I add SNS to my CloudWatch Alarm
To do that I will go into my CloudWatch
From there I go into my Log groups
In it I should see the log streams
Then I will enter into Metric filter
So, I go to CloudWatch-Log groups
In it I go to Metric filters
There I select a Metric filter
Next I create an Alarm -In CloudWatch-Log groups
Select Metric
In it I go to Browse and select vpc flowlogs
And then In Alarm I selected Period 1 minute for simplicity.
In Condition in Alarm I selected Static
Greater/Equal
And defined the threshold value by 1
And click next
Then in Create a new topic window I typed – Whenever-reject-traffic-happens
Email endpoints that will receive the notification – type – temelkaya@hotmail.com
Then I click on Create topic
In or der it to work, I have to go to my email and confirm it.
Then in CloudWatch – Alarms – Create alarm section – I give a name to Alarm.
I name Alarm SSH rejects and click Next
And create an Alarm
