# Manual Scheduling
give it a nodeName for a pod when first created
![sc2](../../images/sc2.PNG)  add nodeName to manual assign node

# Labels & Selectors
check label
kubectl get pods --selector env=dev 
kubectl get all --selector env=prod   (for all objects)
kubectl get all --selector env=prod,bu=finance,tier=frontend   (no space)

# Taints(Node) and Tolerations(Pod)
scheduler places pods to nodes -- nodes has taints eg.=blue -- only allow those with certain tolerations eg.=blue, can satisfy with that taints on the node

- kubectl taint nodes <node-name> key=value:taint-effect
eg. kubectl taint nodes node1 app=blue:NoSchedule
The taint effect defines what would happen to the pods if they do not tolerate the taint.
- There are 3 taint effects
  - **`NoSchedule`** 
  - **`PreferNoSchedule`** --try to avoid place pods but not sure
  - **`NoExecute`**
  ![tp](../../images/tp.PNG)
  
for master node, default has taint and not allow any pod with container
to check  --> kubectl describe node kubemaster | grep Taint
    
# Node Selectors
To label nodes
$ kubectl label nodes <node-name> <label-key>=<label-value>
$ kubectl label nodes node-1 size=Large
So a pod definition --> nodeSelector size=large
  ```
  apiVersion: v1
  kind: Pod
  metadata:
   name: myapp-pod
  spec:
   containers:
   - name: data-processor
     image: data-processor
   nodeSelector:
    size: Large
  ```

# Node Affinity
advanced pod definition to select node
![na](../../images/na.PNG)  operator -- In/NotIn/Exists
## Node Affinity Types
pod state - duringScheduling -- deploying the first time -- definately deploying based on rules
pod state - duringExecution -- if changed label name in the future, but the pod alreday deployes to node
Ignore -- as soon as it's deploying the rule is ignored
- Available
  ![nats](../../images/nats.PNG)
- Planned
  ![nats1](../../images/nats1.PNG)

# Resource requiremtn
kube-scheduler place pods to nodes also depend on resource
## cpu
1 = 1 AWS vCPU = 1 GCP Core = 1 Azure Core = 1 Hyperthread
0.1=100m 
## memory
1G/gigabyte = 1M/megabyte *1000 = 1K/kilobyte *1000 = 1000 bytes
1Gi/gibibyte <-*1024 1Mi/mebibyte * <- 1ki/kibibyte = 1024 bytes
256 Mi - mebibyte

OOM - out of memory