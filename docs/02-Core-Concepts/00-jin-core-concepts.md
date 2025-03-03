# Cluster Architecture
  docker - container runtime
  k8s - container orchistrator, host app in form of containers
  nodes can be physic/virsual, on premise/cloud

## Nodes
(cargo)**worker nodes** : host app in form of container
  contaniners need to be compatible --> container runtime engine: *docker/rkt* --> need to be installed on nodes
  *kubelet* - listen instruction from apiserver
  *kube-proxy service* : allow containers to reach/communiate each other

**master node** : manage/plan/schedule/monitor nodes
  *etcd cluster*: db store info in key-value format
  (crans)*kube-scheduler* : load containers in ship based on size/capacity...
        Decide which pod goes on which node - depend on container/pod requirement (CPU/memory) - place in right node (node ip)
  *kube controller manaager*
    *Node-controller* - Apiserver - Check nodes health -> unhealthy(mark as unreachable -> remove pods on that node and provision a new pod on a healthy node)
    *Replication-controller*/*Replica Set* → apiserver - make sure replica sets, there are certain number of pods are available all time
  *kube-apisever* : orchistrate all communications
    Kubect command - api server - auth - etcd search
      Kuberserver -(create pod) > kubelet -> call docker to pull image and run instance - kubelet update pod status and send back to kubeserver
    Post request - kuke-apiserver - auth - update ETCD cluster
    Scheduler continuous monitor api server - if there is a pod without node assigned - check and pass to kubeket - deploy node → update etcd again

## Yml file
  apiVersion - kubeapi version
    Pod v1
    Service v1
    Replicaset apps/v1
    Deployment apps/v1
  Kind - type of object
  Metadata
  spec

K8S deployment
  Roling upgrade

******************************************

# Docker vs. ContainerD

docker (cli/api/build/volumes/auth/security)--> dockershim -- > k8s
containerd (*nerdctl* cli) --> CRI --> k8s

other container runtime --> obey OCI-open container initiative(imagespec+runtimespec) --> call CRI container runtime interface --> (*crictl* cli)deploy to k8s

k8s - container orchistrator, host app in form of containers

****************************

# ETCD for Beginners
key-value
  tabular/relational db --> row and column
  key-value store --> each individual is a document or file

******************

# ReplicaSets
Replication controller
    multi pods share requests -- load balancing/scaling
    high availability -- control num of instances running
    replication controller spec file ![rc2](../../images/rc2.PNG)   
    --> kubectl create -f rc-definition.yml  kubectl get replicationconroller
--> advanced version: **Replica Set**
    ![rs](../../images/rs.PNG) add selector defination
    when create pod, give it a label, so when define replica set, can use that label to know which pods need to maintain certain running instances
    --> kubectl get replicaset
    when update number of replicas
        --> kubectl replace -f replicaset-definition.yml
        --> scale commands

# Deployments
kubectl get all --> to see all deployments

# Services
connection between user and pod --> between pods - connect to external data source
NodePort Service
    listen to port on a node --> forward request to pod running the app through port
    ![srvnp](../../images/srvnp.PNG)
ClusterIP
LoadBalancer