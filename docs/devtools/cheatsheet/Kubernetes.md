# Kubernetes Commands

```bash

Kubernetes basic Objects

- pod : Basic execution unit of a k8s applicaiton
- service : An abstract way to expose an app runnig on set of pods as network service.
- volume : A volume outlives any containers that run within the pod.
- namespace : Namespaces provide a scope for names

Kubernetes higher level abstractions

- deployment
- daemonset
- statefulset
- replicaset : Specified number of pod replicas are running at any one time. 
- job

```

- Basic commands

### nodes

```bash

# get the list of nodes
> kubectl get nodes

# get details of a selected node
> kubectl describe node <node name>

# delete a node
> kubectl delete node <node name>


```

### namespace

```bash

# get the list of namespaces
> kubectl get namespaces
> kubectl get namespaces
> kubectl get ns

# create a new namespace
# > kubectl create namespace <namespace name>
> kubectl create namespace ns1

# delete a namespace
# this command will also delete all the objects under the namespace
# > kubectl delete namespace <namespace name>
> kubectl delete namespace ns1


```

### pod

```bash


# get the list of pods running in default namespace
> kubectl get pods

# get the list of pods running in requirement namespace
> kubectl get pods -n <ns name>

# get the list of pods with wide/more options
> kubectl get pods -n <ns name> -o wide

# create a pod using pod1.yaml file
> kubectl create -f pod1.yaml

# get the details of selected pod
> kubectl describe pod <pod name>

# delete the pod from default namespace
> kubectl delete pod <pod name>

# delete the pod from required namespace
> kubectl delete pod <pod name> -n <ns name>

# get the logs of a selected pod
> kubectl logs <pod name>

# get the logs continuously of a selected pod
> kubectl logs -f <pod name>

# execute a command inside a pod
> kubectl exec -it <pod name> -- <command>

# get the terminal of a selected pod
> kubectl exec -it <pod name> -- bash

# get the terminal of a selected pod from a selected containers
# if -c is not given, the first container will execute the command
> kubectl exec -it <pod name> -c <container name> -- bash

```

### replica sets

- used to create multiple replicas of selected pod

```bash


# get list of replica-set
> kubectl get replicasets
> kubectl get replicaset
> kubectl get rs

# get details of selected replica-set
> kubectl describe rs <rs name>

# to scale out or in, update the replicas in yaml file
> kubectl apply -f <rs yaml file>

# delete a replica-set
> kubectl delete replicaset <rs name>

```

## service

- used to balance the load amongst multiple pods
- these multiple pods can be created using replica-set or deployment
- types
  - ClusterIP
    - service which will be accessible only within the cluster
    - service can not be accessed outside the cluster
    - can be used to access an application inside the cluster by other pods
    - e.g. frontend pod is accessing backend service which is load balancing the backend pods
    - ports
      - port
        - the internal client will send the request to service on this port
        - you are free to choose this port as per your requirement
      - targetPort
        - service will forward the request to pod(s) on this port
        - this port number must be same as the port on which the pod is listening on
  - NodePort
    - service will make the application accessible outside the cluster
    - it internally will create a clusterIP service
    - ports
      - port
        - the internal client will send the request to service on this port
        - you are free to choose this port as per your requirement
      - targetPort
        - service will forward the request to pod(s) on this port
        - this port number must be same as the port on which the pod is listening on
      - nodePort
        - the port assigned to the node on which external client will send the request
        - if needed you can specify the nodePort within the range of 30000-32767
        - if not specified, the kubernetes will assign a random nodePort to the service
  - LoadBalancer
    - used to create a load balancer in cloud (for AWS it will create ALB)

```bash

# get the list of services
> kubectl get services

# get the service details
> kubectl describe service <service name>

```

## config map

- collection of key-value pairs (configuration)
- used for storing non-sensitive application configurations
  - e.g. port number, backend url
- all the configurations stored in config map are exposed to the application
  via environment variables
- all values must be in string format (wrapped in double quotes)

```bash

# get the list of config maps
> kubectl get configmap
> kubectl get cm

# get details of selected config map
> kubectl describe cm <cm name>

# delete selected config map
> kubectl delete cm <cm name>

```

## secrets

- collection of key-value pairs (configuration)
- used for storing sensitive application configurations
  - e.g. password, secret, access token
- all the configurations stored in secrets are exposed to the application
  via environment variables
- all values must be in bas64 encoded string format (wrapped in double quotes)

```bash

# get the list of secrets
> kubectl get secrets

# get details of a selected secret
> kubectl describe secret <secret name>

# delete selected secret
> kubectl delete secret <secret name>

```

## deployment

- represents logical deployment of an application
- internally it uses replica set to replicate the pods
- can be updated or rollbacked using rollout commands

```bash

# get the list of deployments
> kubectl get deployments
> kubectl get deploy

# get details of selected deployment
> kubectl describe deploy <deploy name>

# delete deployment
> kubectl delete deploy <deploy name>

```

## rollout

```bash

# restart the deployment using rollout
# this will force deployment to load the new version from docker hub
> kubectl rollout restart deployment <deployment-name>

# get the history of rollout
> kubectl rollout history deployment <deployment-name>

# rollback to the older version (previous version)
> kubectl rollout undo deployment <deployment-name>

# rollback to the specific older version
> kubectl rollout undo deployment <deployment-name> --to-revision=<version-number>

# update the image tag (version)
> kubectl set image deployment <deployment-name> <container-name>=<newer version>

# get the current status of rollout
> kubectl rollout status deployment <deployment-name>

```

## persistent volumes

```bash

# get the list of persistent volumes
> kubectl get persistentvolumes
> kubectl get pv

# create a pv
> kubectl apply -f pv.yaml

# get details of selected pv
> kubectl describe pv <pv name>

# delete a pv
> kubectl delete pv <pv name>

```

## persistent volume claim

```bash

# get the list of pvc
> kubectl get pvc

# get details of a selected pvc
> kubectl describe pvc <pvc name>

# delete a pvc
> kubectl delete pvc <pvc name>

```

## metrics service

```bash

# apply the metrics server yaml
> kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml

# the above url will deploy the metrics server in kube-system namespace
# by default, this will not work
# to fix the problem
> kubectl edit deployment metrics-server -n kube-system
# add the following line on line number 45
> --kubelet-insecure-tls=true
# save and exit => esc :wq

# get the top node usage
> kubectl top nodes

# get the top pods usage
> kubectl top pods

# get all the resources created in the kube-system namespace
> kubectl get all -n kube-system

```

## horizontal pod autoscaling

```bash

# get the list of hpa
> kubectl get hpa

# get details of selected hpa
> kubectl describe hpa <hpa name>

# delete a selected hpa
> kubectl delete hpa <hpa name>

```

## job

```bash

# get the list of jobs
> kubectl get jobs

# get details of selected job
> kubectl describe job <job name>

# delete a selected job
> kubectl delete job <job name>

```

## cron job

```bash

# get the list of cronjobs
> kubectl get cronjobs

# get details of selected cronjob
> kubectl describe cronjob <cronjob name>

# delete a selected cronjob
> kubectl delete cronjob <cronjob name>

```