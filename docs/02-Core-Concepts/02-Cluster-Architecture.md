# Cluster Architecture
  docker - container runtime
  k8s - container orchistrator, host app in form of containers
  nodes can be physic/virsual, on premise/cloud

(cargo)**worker nodes** : host apps as containers
  contaniners need to be compatible --> container runtime engine: *docker/rkt* --> need to be installed on nodes
  *kubelet* - listen to api server
  *kube-proxy service* : allow containers to reach/communiate each other

**master node** : manage/plan/schedule/monitor nodes
  *etcd cluster*: db store info in key-value format
  (crans)*kube-scheduler* : load containers in ship based on size/capacity...
  *kube controller manaager*
    node-controller : nodes start/destroyed...
    replication-controller
  *kube-apisever* : orchistrate all communications

K8s Reference Docs:
- https://kubernetes.io/docs/concepts/architecture/

![Kubernetes Architecture 1](../../images/k8s-arch1.PNG)