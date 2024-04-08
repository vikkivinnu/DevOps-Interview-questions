AWS services
-----------------
subnet --> VPC
SG --> VPC
LB --> VPC
VPC level and NON VPC level resources

route53 --> non VPC level resource

K8
----------------
namespace --> true/false

Pods
----------------
pods are smallest deployment unit in k8

pod vs container
---------------------
1. 1 pod can have multiple containers
2. all containers inside pod share same storage and n/w

docker run -d -p 80:80 --network nginx:latest

we are not using docker to run images, we are going to use k8 to run images

multiple containers can be used in
1. side car
2. proxy patterns
3. init containers

labels
--------------
lables are used in selectors, it has some functional advantage apart from filtering.

labels vs annotations
--------------
labels can't have special char in key names, annotations can have
lable key have some length restrictions, annotations length can be more than lables

labels are used in kubernetes resources selectors
annotations are used in selecting external resources

VM and docker
-------------
1. resource utilisation

1Gb ram and 5gb hd --> blocked in VM
can dynamically consume resources --> not blocking system resources

app locks --> can consume more memory because of code defect
container can block complete system resources, we can implement resources limits in docker

resources are not blocking the memory and ram, k8 is just monitoring pods should not consume more resources than mentioned.

requests: # soft-limit 
        memory: "64Mi"
        cpu: "250m"
limits: # hard-limit
memory: "128Mi"
cpu: "500m"

1 cpu == 1000m
250m = 0.25 cpu

image pull policy
------------------
pushing images to docker hub
we push roboshop images to docker hub

we are running kubernetes pod --> already pulled image(default)

developer did some change, he pushed new image to docker

we run kubernetes pod again like kubectl apply -f pod.yaml


config-map
----------------
keep the config away from container..

URL --> configmap
username/password --> secret


services
----------------------
1. ClusterIP --> internal to cluster, not exposed to outside world, only exposed to inside cluster
2. NodePort
3. LoadBalancer --> cloud related kubernetes

1. to expose your application to outside world
2. service mesh
2. to balance the load --> deployment and replicaset

==========================
on-demand instances --> you will get the servers for sure

spot instances
-----------------
10,000 CPU in mumbai
8000 CPU, 2000 CPU is free

spot instaces --> 80% discount
at any point of time, AWS can take back these resources, 2min notice

SETS
----------------
1. Replica set
2. Deployment set
3. daemon set
4. statefulset

when you provision user, you change nginx conf and include user reverse proxy
push changes to git
do docker build --> build and push again to docker hub
restart the container

when the code is changed then only go for build,
if config changed, you should not go for image build, you should restart

we can keep config files also in config map...

===========
Pod
ConfigMap
Service
ReplicaSet
Deployment
Secret

Storage
----------------
worker nodes are ephemeral.
pod itself is ephemeral, anytime it may be deleted. if we store data inside pod we lose it.
we created docker volumes and mounted to container.

1. emptyDir --> ephemeral, inside pod
2. hostPath --> ephemeral, inside server

3. static provisioning --> external and perm
4. dynamic provisioning --> external and perm

pod vs container
--------------
1. multi containers in pod
2. they share same storage and network

filebeat is popular log shipping model from kubernetes to elastic search
it needs to have some config.
	1. elastic search address --> where to ship
	2. it needs what are the files to ship
we will provide this configuration through configmap

emptyDir is a kubernetes ephemeral volume used in sidecar patterns, we will mount volume to main container and sidecar container so that they can share storage. main container wll write logs sidecar container will access and ship logs to elastic search

we use hostPath generally to ship server logs

deamonset
------------
if you run deamonset kubernetes will create a pod in each and every worker node.

hostPath will be only done by admins to ship server logs to elastic search with deamonset.

deamonset --> fluentd --> a shipper to access and send log files to elastic search
deamonset will access logs through hostPath

1. create volume --> persistance
2. mount volume --> giving access to pods

deamonset --> make sure a pod runs on every worker node
hostPath --> to give access of server storaqe to the pod
fluentd --> ship log files to elastic search


static provisioning
----------------------
hard disk --> sits next to computer, store files in harddisk

EBS --> elastic block store

create one EBS volume, give access to EKS cluster.

EKS pods will store data in EBS volumes...

now pods or servers may be deleted, but your data is not deleted, you can mount again to another pod so that pod can access the data.

storage and backup admin --> netapp

persistant volume --> this a representation of underlying storage..
persistant volume claim --> pod will ask for storage.
storage class --> dynamic provisioning

We will create EBS disk -> inside cloud
We will create equivalant pv object to represent EBS disk..

something external to computer we should install drivers.
	- sound
	- keyboard
	- mouse
	- usb

install EBS drivers.

steps:
----------
1. create disk
2. install drivers
3. create PV
4. make sure EC2 can access EBS
5. create PVC --> a way of claiming storage
6. create volume out of PVC
7. mount to container

EC2 --> EBS

3 worker nodes

10 pods 
3 pods --> ip-192-168-51-12.ec2.internal --> EBS volume
7 pods --> diff server

EFS --> elastic file storage
------------------------------
NFS --> network file storage



User
Orders data
cities and countries
payments

-------
MySQL
rabbitmq
mongodb
redis
===========
emptyDir --> ephemeral --> used in sidecar patterns to share the storage between containers in the pod
hostPath --> gives access to host storage to get the log files generally, used in deamonset to get server logs

perm volumes
--------------------
static provisioning
dynamic provisioning

EBS ->

1. created EBS volumes
2. install drivers EBS CSI
3. give access to worker nodes
4. created PV
5. created PVC
6. mounted to the pod

Dynamic
-----------
creation of disks can be handled by Kubernetes.

StorageClass --> disk and PV --> admin activity

EKS cluster --> roboshop, amazon, flipkart

1. install drivers, because SC object use drivers to create volumes.

SC --> k8 admin
PV
PVC --> roboshop engineer create pvc, he mentions which storage class.

storage class will create disk and pv automatically.


EBS vs EFS
-------------
ebs is block store, EFS is network storage
ebs can't scale automatically. EFS can get more disk space automatically.

EFS static provisioning
---------------------
1. create EFS file system
2. edit the security group to allow traffic
install drivers
3. give access to ec2 to access efs
4. create pv, pvc and mount to pod

kubectl kustomize \
    "github.com/kubernetes-sigs/aws-efs-csi-driver/deploy/kubernetes/overlays/stable/ecr/?ref=release-1.7" > private-ecr-driver.yaml
	
EFS dynamic provisioning
-----------------------
1. create EFS file system
2. edit the security group to allow traffic
install drivers
3. install drivers
3. give access to ec2 to access efs

a single file system can be used by entire company

1. we will create sc for each project


statefulset
----------------------
ReplicaSet
DeploymentSet --> stateless applications
DeamonSet
StatefulSet --> DB related applications(MySQL, Redis, MongoDB, RabbitMq)

DeploymentSet vs StatefulSet
---------------------------
DB related applications

need to have same name and identity
statefulset will create pods in ordery manner, nginx-0, nginx-1, nginx-2 will be created orderly. deployment can create many pods at a time
statefulset keeps the pod identity same for the communication.
statefulset preserve network identity

deployment will use service, but statefulset it is mandatory to use headless service...
1. deployment and attatch it to service
	in deployment nslookup nginx-service will give IP address of service
2. in statefulset nslookup nginx-service
====================
RBAC
----------
Role based access control
Authentication and Authorization

Role
RoleBinding
ClusterRole
ClusterRoleBinding

Pod --> namespace
Service --> namespace
Deployment --> namespace
StatefulSet --> namespace
PV --> ClusterLevel
PVC --> namespace
ConfigMap --> namespace

either namespace level or cluster level

EKS --> Roboshop, Amazon, Flipkart

EKS Admins --> full access to entire cluster

Roboshop
--------
roboshop-admin --> devops and cloud engineers
roboshop-developers --> roboshop developers limited access

Roboshop
-----------
1. project started
2. team leadres, managers or devops guys
3. create namespace and provide access

As EKS admin
-----------
1. create namespace

AWS is cloud, EKS is platform
--------------------------
IAM --> Authentication
K8 RBAC --> Authorization

ramesh --> should describe this cluster eks-spot-cluster

Role --> permissions

trainee --> read
junior engineer --> 
senior engineer
team leader --> module admin
manager --> entire project admin
MD --> account admin


sivakumar --> trainee
ramesh --> team leader

configmap aws-auth in kube-system, here we need to map IAM user and k8-rbac


apiVersion: v1
data:
  mapRoles: |
    - groups:
      - system:bootstrappers
      - system:nodes
      rolearn: arn:aws:iam::315069654700:role/eksctl-eks-spot-cluster-nodegroup--NodeInstanceRole-ZFRyOLy0FVrN
      username: system:node:{{EC2PrivateDNSName}}
kind: ConfigMap
metadata:
  creationTimestamp: "2023-11-21T01:43:14Z"
  name: aws-auth
  namespace: kube-system
  resourceVersion: "8642"
  uid: 94eacb41-7c0a-4038-91cf-d6ad800e1867



HorizontalPodAutoscaler
-------------------------
replicas --> 1 or 10

CPU Utilisation or Memory Utilisation


metricsserver
------------

Ingress Controller
------------------
amazon.joindevops.online --> it can go to amazon project
flipkart.joindevops.online --> it can go to flipkart project

m.facebook.com --> redirect mobile website
facebook.com --> actual application
admin.facebook.com --> redirect admin website

joindevops.online/amazon

joindevops.online/flipkart
========================
