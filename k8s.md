# .................. Kubernetes Interview Questions  ....................


#### \[1\] So, what have you done with Kubernetes? This question comes up all the time!

##### Answer:

While this seems easy, a prepared and practiced answer is significantly better than an impromptu one.

Your answer would be unique to your experience, but, here are some possibilities.

- a. created clusters
- b. upgrade master and nodepool versions
- c. upgraded legacy monitoring 
- d. added Istio (Kiali)
- e. added weave
- f. deployed via helm charts
- g. deployed spinnaker 
- h. configured HPA
- i. day to day: configmps, secrets, PVs, PVCs
- j. troubleshot operational issues
- k. stateful sets 
- l. created CSRs and signed certificates

## ........

#### \[2\] You have 2 different contexts (A and B). Context A has a secret named foo. Context B does not. What would be a quick way to create the same exact secret in Context B?

Answer: 
1. Switch to Context A
2. kubectl get secret foo -o yaml > foo.yaml
3. Switch to Context B
4. kubectl apply -f foo.yaml

## ...



## ......

#### \[3\] There is more than one way to implement Ingress? What did you use to implement Ingress?

Answer: Ingress is IMPLEMENTED by Ingress Controllers. There are at least 12. Most common is a Load Balancer (GCP/AWS). Another popular one is Nginx Ingress Controller. See below for a longer list. (Question #8)

## ...



## ......

#### \[4\] Why do we need Kubernetes? What problems does it solve?

Answer: As soon as we decide to use docker/container as a platform, we run into new issues such as:
- a. orchestration
- b. inter-container communication
- c. autoscaling
- d. observability
- e. security
- f. persistent and/or shared volumes
and more

Kubernetes solves these problems.

## .


## .....

#### \[5\] What is the difference between Ingress and Ingress Controller:

Answer: Ingress Controller FULFILLS ingress requirements. Defining an ingress has no actual impact on traffic. Traffic is only acted upon once you have created an Ingress Controller (e.g. Load Balancer or Nginx Ingress Controller)

## .


## .....

#### \[6\] Most common type of Ingress Controller?

Answer: Load Balancers

## .


## .....

#### \[7\] Kubernetes as a project supports and maintains which 3 Ingress Controllers?

Answer: AWS, GCE, and nginx ingress controllers. (This is straight from Kubernetes documentation)

## .


## .....

#### \[8\] Besides those 3, what other ingress controllers are there?

Answer: From: https://kubernetes.io/docs/concepts/services-networking/ingress-controllers/

a. HAProxy
b. Istio Ingress
c. Traefik kubernetes Ingress Provider
d. Skipper
e. Voyager
f. Tyk Operator
g. Gloo (open source)
h. AKS Application Gateway Ingress Controller (Azure)
i. Ambassador (envoy-based)
j. Enroute (another envoy-based Ingress Controller)
(and more)

## .


## .,...

#### \[9\] How would one start up a Kubernetes cluster to deploy containers/pods on (in GCP)?

Answer: 
a. via GCP GUI  OR
b. via GCP cloud shell window OR
c. gcloud CLI
d. Terraform
e. Google Deployment Manager

## ...



## ......

#### \[10\] If a container keeps crashing, how do you troubleshoot?

Answer: You can use --previous option with logs command to see the logs of a crashed container. (kubectl logs --previous)

## .


## ......

#### \[11\] What happens to containers if they use too much cpu or memory?

Answer: if they use too much memory, they are evicted. if they use too much cpu, they are throttled.

## .


## ......

#### \[12\] How do you manage scaling in Kubernetes?

Answer: 
This article answers it very well: https://www.replex.io/blog/kubernetes-in-production-best-practices-for-cluster-autoscaler-hpa-and-vpa

a. hpa for pods (horizontal pod autoscaler)
b. vpa for pods (vertical pod autoscaler)
c. Cluster Autoscaler:
  The cluster autoscaler is a Kubernetes tool that increases or decreases the size of a Kubernetes cluster (by adding or removing nodes), based on the presence of pending pods and node utilization metrics

## ...



## ......

#### \[13\] How have you used RBAC with Kubernetes ?

Answer: Answer will depend on your use case. One possible answer is to have Service accounts that do certain things within the cluster. By the way, RBAC in Kubernetes is just AWS IAM Policies and Bindings. In RBAC, you have subjects (who gets the permission), verbs (what can the subject actually do), and rolebinding (subject linking to roles) and roles.

## .


## ......

#### \[14\] If you have 200 micro-services in your clusters, how do you manage the security of each one? How do you avoid toil?

Answer: RBAC is the answer. You define roles. And you place subjects in those roles. Each role then will have access to X Y Z etc. This is really no different than AWS or AD.

## .


## ......

#### \[15\] Tell me about the hardest production Kubernetes issue you solved or faced?

Answer: Your answer will be unique to your experience. But, here is a hypothetical answer.

There are N micro-services. One of them gets a new version. But, the HPA for those pods are set wrong. Container keeps crashing. This causes cascading failures for many other micro-services. Solution: Fix the HPA settings and add circuit-breakers in the consuming microservices.

## .


## ......

#### \[16\] You want to know how to make yaml files for making PODs and you have no access to the internet. What do you do?

Answer: kubectl explain pod --recursive

It will show you all fields in a mapped kind of format so you exactly what field goes where

Similarly: kubectl explain pv --recursive (for PVs)

## .

## ......

#### \[17\] How can you have SSL certificates in Kubernetes?

Answer: SSL cert can be a secret. Then that secret can be mounted on a pod and that pod can do whatever it wants with it (e.g. host a SSL website)

## . 

## ......

#### \[18\] Open Source Tool to switch contexts easily:

Answer: kubectx

## . 

## ......

#### \[19\] Opensource menu-driven text-based flexible Tool to manage Kubernetes everything:

Answer: k9s

## . 

## ......

#### \[20\] "kubectl explain" command is great, but you must know the exact name of the resource (e.g. pod/services/persistentvolume) to get the details, unless you do recursive. How do you get the names of these resources from the command line?

Answer: kubectl api-resources (gives you a list and short names and more)  

## .

## ......

#### \[21\] Name some of the other verbs that kubectl has besides "run" "create" or "apply"?

Answer: There are many! Some examples below:

expose, set , explain, get, edit, delete, rollout, scale, autoscale certificate, cluster-info, top, cordon, uncordon, drain, taint describe, logs, attach, exec, port-forward, proxy, cp, auth, debug diff, apply, patch, replace, wait, kustomize, label, annotate, completion, api-resources, api-versions, config, plugin, version

Some of the more frequently used ones are: logs, get, port-forward and label.

## .

## ......

#### \[22\] What might you get when you run kubectl api-resources? 

Answer: api-resources is a fancy term. Basically you get stuff like pods/secrets/config-maps all that stuff.

## .

## ......

##### \[23\] How else can you get help with kubectl? (besides kubectl explain command)

Answer: kubectl --help  is actually better than kubectl in my opinion.

## .

## ......

#### \[24\] You ran "kubectl --help" but you want a little more help. What to do?

Answer:
kubectl get --help 
kubectl top --help 
kubectl describe --help 

## .

## ......

#### \[25\] Outline the steps to deploy additional scheduler on a Kubernetes cluster (not GKE)

Answer:
Package the new scheduler in a docker image
Put that image in a registry
Create a deploymentment file with type: deployment and component: scheduler (in namespace kube-system)
Deploy the the scheduler with apply -f scheduler.yaml command

## .



## ......

#### \[26\] List out 2 use cases for Daemonsets and explain why it is more appropriate to use daemonset than deployment for those use cases:

Answer:
1. Pod that collects logs. Better to use daemonsets for this because you can logs to be fed from all pods (e.g. to kibana). Otherwise you have to make this part of EVERY deployment which would be annoying and repetitive.
2. Pod that runs monitoring (e.g. dynatrace or datadog). Reason is the same as above.

## .



## ......

#### \[27\] How to move workload to the new nodepool? 

Answer: cordon and drain 1. cordon means: don't add any more pods to this nodepool 2. drain means: move current pods out of it

## .  

## ......

#### \[28\] Is ClusterIP private or public?

Ans: Private

## .

## ......

#### \[29\] Which one will allow you to access your services from the internet: cluster ip or nodeport? 

Answer:  nodeport. confirmed!  why? Because NODE is a VM with an external IP and thus can be reached. Cluster IP is 10.x IP (internal)

## .

## ......

#### \[30\] For a service, when we use nodeport, EVERY node does what?

Answer: Gives that service an IP and proxy it. confirmed 

## .


## ......

#### \[31\] What does it mean when we say that a node proxy's a service?

Answer: The node forwards the traffic to a pod that is part of the service.

## .


## ......

#### \[32\] 2 ways to let container have access to a secret: 

Answer: Volume and ENV variable

## .


## ......

#### \[33\] How can a container have access to secrets via the ENV variable? 

Answer: You can define a ENV in yaml file just like everything else and container can just do echo $WHATEVER

## .

## ......

### \[34\] One-liner kubectl command to run a pod with nginx:alpine 

Answer:  k run nginx-pod --image=nginx:alpine  (nginx-pod is arbitrary pod name)

## .

## ......

#### \[35\] One liner command to  run a pod with a label  

Answer: kubectl run foobar --image=redis:alpine -l label1:foo 

## .


## ......

#### \[36\] kubectl command to show labels of all pods in the default namespace:  

Answer: kubectl get pods --show-labels 

## .

## ......

#### \[37\] Whenever you run a kubectl command, it runs in the default namespace. How do you make it run in a different namespace?

Answer: use -n namespace_name   (to whatever kubectl command you are running.)

## .

## ......

#### \[38\] Command to create a namespace: 

Answer: kubectl create ns foobar # create a namespace

## .

## ......

#### \[39\] When using the kubectl command, how do you get output in json format?  

Answer: kubectl get nodes -o json # json format

## .

## ......

#### \[40\] kubectl expose command: port VS targetport:  (which one is which ?)

Answer:
port : on the cluster
targetport: on the container (just like ALB)

## .




## ......

#### \[41\] Command to expose a pod as a service 

Answer: kubectl expose pod foobarpod --name foobarservice --port 6379 --target-port 6379  # expose a pod as a service
            (NOTE servicename is specified: foobarservice)

## .



## ......

#### \[42\]  Command to get details of a service : 

Answer: kubectl describe svc foobarservice # get details of that service

## .



## ......

#### \[43\] Command to create a deployment from image: foobar/webapp-color 

Answer: kubectl create deployment foobardeployment --image=foobar/webapp-color 

## .



## ......

#### \[44\] Command to scale deployment named foobardeployment to 2 replicas  

Answer: kubectl scale deployment foobardeployment --replicas=2 # scale that to 2 replicas  

## .



## ......


#### \[45\] Can you scale a kubernetes service?

Answer: No. You can scale deployments and replica sets

## .


## ......

#### \[46\] If you want your kubernetes command to have a scope of ALL namespaces, how do you do that?

Answer: add -A to the command

## .




## ......

#### \[47\] Are environment variables encrypted in Kubernetes? 

Answer: No

## .




## ......

#### \[48\] By default, can a pod in one namespace talk to another pod in another namespace? 

Answer: Yes.

## .



## ......

#### \[49\] How to generate a yaml file from an imperative command you know works ? 

Answer: add: --dry-run=client
