# Kubernetes cheatsheet 
Install jq
`sudo apt-get install jq -y`


## Namespaces


> To list all the namespaces

`kubectl get ns`

> To permanently set a namespace as default

`kubectl config set-context $(kubectl config current-context) --namespace=<namespace>`

Example: To set `asr` namespace as default:

`kubectl config set-context $(kubectl config current-context) --namespace=asr`


## Workload controllers

> To list all daemonsets a namespace

`kubectl get daemonsets -n <namespace>`

> To list all deployments a namespace

`kubectl get deployments -n <namespace>`

> To list all ReplicaSets in a namespace

`kubectl get rs -n <namespace>`


## Pods

> To list pods in a namespace

`kubectl get pods -n <namespace>`

> To get complete details of pods

`kubectl describe pod <pod-name> -n <namespace>`

> To know which deployment/replicaset is controlling this pod:

Sometimes even after deleteing the pod, a new one would automatically be generated. To know who is creating/controlling these pods :

`kubectl get pods -o json -n <namespace> | jq '.items[] | select(.metadata.name=="<pod-name>")' | jq '.metadata.ownerReferences'`

> To list pods in all namespaces

`kubectl get pods --all-namespaces`


> To list container names of a given pod

`kubectl get pods -o json -n <namespace> | jq '.items[] | select(.metadata.name=="<pod-name>")' | jq '.spec.containers[].name'`


> To get IP address of a pod

`kubectl get pods -o json -n <namespace> | jq '.items[] | select(.metadata.name=="<pod-name>")'  | jq '.status.podIP'`


> To get logs of a pod

If there is only one container in the pod :

`kubectl logs <pod-name> -n <namespace>`

If there are multiple containers in the pod :

first get the name of the container, whose log you want to inspect.

`kubectl logs <pod-name> -c <container-name> -n <namespace>`

If you want to continuously tail the logs, use `-f` flag. For example,

`kubectl logs -f <pod-name> -c <container-name> -n <namespace>`


> To get readinessProbe of a pod

Sometimes pods keep restarting. Logs are hard to capture in restarting pods. To know which command is failing in the pod's checks, use the following command to get the readinessProbe

`kubectl get pods -o json -n <namespace> | jq '.items[] | select(.metadata.name=="<pod-name>")' | jq '.spec.containers[] | select(.name=="<container-name>")' | jq '.readinessProbe'` 


> To execute a command in a running pod.

`kubectl exec -it <pod-name> -c <container-name> -n <namespace> -- <command>`

More specifically, to get a bash prompt of a container

`kubectl exec -it <pod-name> -c <container-name> -n <namespace> -- /bin/bash`

Sometimes, "bash" will not be present in container. use sh instead

`kubectl exec -it <pod-name> -c <container-name> -n <namespace> -- /bin/sh`


# Services

There are three types of services that we majorly see time and again
- ClusterIP
- LoadBalancer
- NodePort

### NodePorts
The service reserves a port in *every* node for itself,  which is in the range 30000-32767

### LoadBalancer
This type is exclusively for CloudProviders, not on-prem k8s setup. It creates
a loadbalancer in the cloud-platform and directs traffic to the service

### ClusterIP
Exposes an IP that is internal to the kubernetes cluster


> To list services

`kubectl get svc -n <namespace>`


> To list services in all namespaces

`kubectl get svc --all-namespaces`


> To get details of a service

`kubectl describe svc <service name> -n <namespace>`

> To edit a service, possibly for changing its type from clusterIP to
> LoadBalancer, etc

`kubectl edit svc <service-name> -n <namespace>`

> To delete a service :

`kubectl delete svc <service-name> -n <namespace>`


# Cluster Role Binding.

> To give a service account cluster-wide admin access:

`kubectl create clusterrolebinding <cluster role binding name> -n <namespace>  --clusterrole=cluster-admin --serviceaccount=<namespace>:<account-name>`


