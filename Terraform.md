Infrastructure
---------------
Infrastructure as a code --> IaaC/IaC
Terraform --> IaaC very popular

ENV setup

advantages
--------------
1. version control
	since terraform is a code, it should be kept in version control, so that we can maintain the history and we can revert to previous version if something goes wrong.
2. consistent infra
	same terraform code you run against multiple env, so all env is exactly same.
3. CRUD --> create read update delete
4. inventory management
	by seeing terraform, you can say what are the services and resources you are using for a particular project.
5. cost optimisation...
	when you need you create infra, when not required you delete the infra
6. dependency management
	Ec2 --> Security group
	first create security group and then create Ec2
	Ec2, sg code
7. modules --> reuse the code..

DRY --> dont repeat yourself
Common --> use it wherever required, central

imperative/declarative

terraform is popular declarative scripting for infrastrcuture automation. easy syntax, no need of sequence execution, we will get it whatever we write if we follow syntax

IDE --> vs code
Terraform.exe keep it in a folder and set the path
AWS CLI V2

Authetication
------------------
create IAM user with admin access
generate access key and secret key
aws configure, give us-east-1

Hybrid IaaC
----------
providers
-----------
AWS
Google cloud
Azure
GitHub

our website
---------
develop providers
terraform can create resources into our website

Roboshop
-----------
create users or signup

you can develop roboshop terraform provider and you can use terraform to create users

creation of Ec2
----------------

HCL --> hashicorp configuration language, very simple

variables
data types
conditions
loops
functions

resource "what-resource" "name-your-resource-your-wish"{
	arguments/options/parameters
}


what-resource --> you need to get from terraform documentation


OS
Key
Security group
Storage

AMI --> mandatory
security_groups --> optional, if you dont provide SG, default SG will selected

creation of resource
----------------------
1. provider --> you need to tell terraform which provider you are using..
2. ec2.tf

terraform init --> this will intialize terraform
terraform plan --> terraform will tell us what are the resources it is going to create
terraform apply --> it will create the resources
github --> code

not to push big files or movies or songs, etc...

variables and datatypes

variable "variable-name" {


}
	
string
number
bool
map
list

1. I will create aws security group

2. I will attach this security group to ec2


type.name-of-your-resource.name

===================================
Universal concepts
----------------------
variables --> inputs
data types
conditions
loops
functions

additional concepts
----------------------
outputs
locals
data sources


output "output-variable-name" {
	value = "value"
}

map

tags --> simple but powerful

key=value

Name=some-name
Environment=DEV/QA/PROD
Terraform=true
Component=MongoDB
Project=Roboshop

conditions
----------
if(expression){
	if true these statements run
}
else{
	if false these statements run
}

expression ? "this will run if true" : "this will run if false"

if MongoDB we are creating t3.medium otherwise t2.micro


loops
-----------
1. count and count index


functions
-----------
I want to crete ec2 instance with key
aws-linux-2
with key
provide public key to the instance

1. create public key
2. create ec2 instance


locals
------------
locals is also a type of variables, but it can have expressions and functions


AMI ID are frequently chaning
AMI ID is different for different environment

data sources --> you can query the data dynamically from source i.e AWS

I want to create security group in default VPC,

you have old resources created manullay, now you are using terraform to create resources and you have dependency on old resources, so you use data source to fetch info dynamically



t2.micro --> t3.medium

t2.micro --> m4.xlarge

1. change the existing resource --> update resource
2. terminate the resource --> create new resource [you lose the data]

if web you need to have public ip
otherwise private ip

for loop
dynamic blocks
terraform state and remote state
terraform.tfvars
=====================
variables and data types
conditions
expression ? "true statements" : "false statements"
count and count index
data sources
locals --> you can run expressions and functions in local
outputs --> to print the output on to terminal

loops
---------------
count and count index --> iterate on list

for each --> map/list

let's create 10 instances using for loop

mongodb and mysql t3.medium
for others t2.micro

for_each when you iterate over a map, it will give us one special variable i.e each

each.key and each.value

dynamic block

count or for each is to create more resources with single syntax

state and remote state
-------------------------
declarative scripting that can create resources based on tf files

declarative = terraform responsibility to create resources whatever you write in tf files

tf files = declaration = ordering terraform

state = terraform has to know what resources it is created

terraform.tfstate = terraform will see this file to understand the resources it created already

tf files = terraform.tfstate

now tf files dont have egress but terraform.tfstate has egress

to delete egress and update terraform.tfstate

terraform is a declarative scripting, terraform responsibility is to create resources declared in tf files, terraform will create terraform.tfstate to understand the resources it created
all the time terraform tries to match tf files with state file


remote state
------------
we have few serious problems if you keep state file in the repo or local folder

devops engineer 1 and devops engineer 2 joined the project

engineer1 --> he will clone
engineer2 --> he will clone

1. duplicate resources
2. error while creating resources saying already exist
3. accidental changes


s3 bucket, you keep your state file and lock with dynamo DB
=======================================
tfvars
------------
Ansible --> config code is same, but how can we use same code to configure multi ENV
hosts and different variables

IaaC
-------------
consistency across multi ENV, we can use same code but different variables

we need 10 instances in DEV and 10 instances in PROD

r53 records
--------
mongodb-dev, mongodb-prod

1. variables.tf --> you can keep default values here, it is not mandatory to keep default values. but you should declare variable.
2. terraform.tfvars --> to override default values inside variables or else no need to keep default values inside, we can directly provide from tfvars file


1. different variable values
2. different state, you need to give different location to store PROD state

multi ENV
----------------
1. maintain different repos
	roboshop-DEV
	roboshop-PROD
	
pros: code is completly different, so that we dotn mess with DEV and PROD
cons: you need to work on multiple repos, code is duplicated in 2 repos

2. same code but different variable and config

pros: code is reused
cons: you should be very careful while chaging, same code is reflecting to PROD also

3. terraform workspaces


Jenkins CICD or manual
-------------------------------

DEV
terraform init -reconfigure -backend-config=DEV/backend.tf

PROD
terraform init -reconfigure -backend-config=PROD/backend.tf


Modules
-----------------------------
DRY --> dont repeat yourself

you create code, reuse it instead of writing same again and again

we will create code --> modules

whenever you need you can always call modules

keep code in one folder, call it from any no of repos

========================================
tfvars
	to replace default values in variables or if values are not specified in variables.tf we can supply from here
	
	to create multi ENV infra, we can control through different variable values and diff state.
	
Module creation
	DRY --> keep the code centralised, you can refer the code as many times as you want.
	
	jar files --> libraries --> you no need to create code from the scratch, you can simply refer from internet
	
	package.json
	
VPC --> virtual private cloud

IPv4 Address --> 32 bits --> 4 octets --> each octet consists of 8 bits

binary --> 2 digits --> 0,1
decimal --> 10 digits --> 0,1,2,3,4,5,6,7,8,9

binary system
--------------

1 bit
--------
_ --> 0

_ --> 1

2 bits
-------
_ _ --> 00

_ _ --> 01

_ _ --> 10

_ _ --> 11

3 bits
-------

_ _ _ --> 000

_ _ _ --> 001

_ _ _ --> 010

_ _ _ --> 011

_ _ _ --> 100

_ _ _ --> 101

_ _ _ --> 110

_ _ _ --> 111


in binary system if you have n bits, you can generate 2^n combinations

32 bits --> 2^32 --> 4.3 billion

2 bits
-------
_ _ --> 00

_ _ --> 01

_ _ --> 10

_ _ --> 11

if I select 1 bit, then how many parts I can divide entire system

we have K parts in total, out of this if you select n bits
parts = 2^n
each part contains --> 2^n-1

k=3=total bits
n=2
parts=2^2=4
parts=2^n=2 parts
each part = 2^(k-n)=2^(3-2)=2

total no of bits = k = 4bits
total numbers = 2^4 = 16 bits

if I select 2 bits = n =2 
how many parts = 2^2 = 4

each part contains = 2^(4-2) = 4 numbers

CIDR --> classless inter domain routing

you get a big n/w --> you need to divide into multiple subnets logically

10.0.0.0 to 10.255.255.255 (10.0.0.0/8)
172.16.0.0 to 172.31.255.255 (172.16.0.0/12)
192.168.0.0 to 192.168.255.255 (192.168.0.0/16)

IP Address = Network ID + Host ID

CIDR notation
----------------
10.0.0.0/8 --> first 8 bits are for Network, remaining 8 bits are for Host

8 bits are reserved, you should not touch 8 bits

24 bits
-----------
I want to divide into 2 parts. if I select 1 bit I can divide into 2^1 = 2 parts


1 st part start with 0 and ends with 01111111


0 position --> 2^0 = 1
1 -> 2^1 = 2
2 -> 2^2 = 4
3 -> 2^3 = 8
4 -> 2^4 = 16
5 -> 2^5 = 32
6 -> 2^6 = 64
7 -> 2^7 = 128

0 -> 127 = 1st part

128 --> 255 = 2nd part

10.0.0.0/9 --> 1st part
10.128.0.0/9 --> 2nd part

10.0.0.0/8 --> VPC CIDR

2 subnets by selecting 1 bit

1st subnet --> 10.0.0.0/9
2nd subnet --> 10.128.0.0/9


AWS will only 16-28 as CIDR notation. Min 16 max 28

10.0.0.0/16 --> 16 digits for N/W and remaining 16 for host
no of hosts = 2^16
k=16
n=8bits
8 bits --> 2^8 --> 256 N/W possible
each part contains 2^16-8 --> 256 possible hosts

subnets
-----------
10.0.1.0/24
 --> web related network 
	10.0.1.0
	10.0.1.1
	10.0.1.2
	
	10.0.1.255
10.0.2.0/24 --> app
10.0.3.0/24 --> db
10.0.4.0/24 --> mumbai web
10.0.4.0/24 --> delhi web



10.0.255.0/24

VPC --> AWS virtual private cloud isolated from other projects, all project resources can be provisioned, we have full control

internet gateway = wifi router

public route 
private route

VPC
Subnets --> 3 subnets
Internet gateway --> attached to VPC
2 route tables --> 1 public and 1 private

associate route tables to subnets

public route table --> public subnet(web)
private route table --> private subnets(app,DB)

VPC
IGW
Subnets
Route table and routes
associations
security groups


================================
VPC

we are going to develop VPC module, so that anyone in our organisation can use this.

High Availability --> create resources in 2 AZ

VPC
IGW
2 public subnets --> 1 1a, 2 1b
2 private subnets --> 1 1a, 2 1b
2 database subnets --> 1 1a, 2 1b
group our database subnets
seperate route table for public
seperate route table for private
seperate route table for database
route table and subnet associations
NAT gateway

module --> terraform-aws-vpc

roboshop-infra --> we will refer the terraform-aws-vpc


ec2 instance

resource "aws_instance" "ec2_instance"

every resource have common tags
	project roboshop
	environment 
	terraform
	
individual tags --> route table
Name --> roboshop-public

2 private subnets
----------------------
module developer, you need to create resource definition

count = 2, you need 2 cidr that should come from user, 2 az that should again come from, merge common tags with private subnet names

2 subnets
aws_subnet.public[0]
aws_subnet.public[1]

aws_subnet.public[*] --> list of subnets created

aws_subnet.public[*].id = ["subnet-01267e3f0a096eb72","subnet-02b6d0911a7eafa35"]

100 teams in a amazon
1 single team can develop modules

100 teams can use it, less efforts

central team can make some best practices and some security.

to reduce the cost central team decided only 2 az...

modules
	1. code reuse
	2. force some best practices and standars according to company
	
1. create your modules

2. use open source modules

================================
odule developer
-----------------
resource creation/definitions are here
variables are must, because diff projects have diff requirements
data, locals, functions
module developer has to give documentation about inputs and outputs


Module user
-----------------
he need to provide variables/inputs according to documentation


vpc module
-----------------
we should only allow user to create resources in 1a and 1b region. he can give any region
we should only accept 2 public/private/database subnets.
let's ask project name from user so that we can give

<project-name>-public-<az>
<project-name>-private-<az>
<project-name>-database-<az>

vpc

any server if it wants to install some packages it needs to connect internet

NAT --> it should elastic IP
ISP will give dynamic IP address
you can request ISP to provide static IP, but they charge like 500-1000 rs


EIP
NAT Gateway
we need to add that as route in private and database route tables

=============================
Terraform
------------
1. develop your own module
2. use open source module

Module development

SG
---------
users can pass the inputs we need to create SG
inbound

1 project --> 1ENV --> 1 VPC

parameter store
-----------------
centrally store the configuration so that any other resource in AWS can refer and reuse

key --> value

open source modules
---------------------

ansible instance through terraform I will write user_data

what is user_data?

creating instance, on successful start of the instance we want to few things, that you can provide through shell script

install ansible
connect to our roboshop instances
run the playbooks

Linux Admin
Manual configuration
Shell automation
Ansible automation
Terraform to create infra automation and ansible to create configuration

Monitoring
------------------
fast responding
handle more traffic
check for errors --> 500 errors, you should monitoring
calculate how many users our application can serve
