# 2. Core Concepts
## KEY
kubectl run podname --image=redis -n finance   (run podname + -n 来表示namespace)

## PODs
create pod using yml
kubectl run redis --image=redis --dry-run=client -o yaml > redis.yaml
cat redis.yaml
kubectl create -f redis.yaml
vi redis.yaml
kubectl apply -f redis.yaml

## ReplicaSets
- Q11/12 改manifest file  --》understand this structure ![rs](../../images/rs.PNG)

## Deployments
最后一题就是用yaml创建deployment
- generate pod manifest yaml file (-o yaml), don't create it (--dry-run=client)
kubectl run nginx --image=nginx --dry-run=client -o yaml

- create a deployment
kubectl create deployment --image=nginx nginx

- create a deployment yaml file (-o yaml), don't create it (--dry-run=client)
kubectl create deployment --image=nginx nginx --dry-run=client -o yaml
kubectl create deployment nginx --image=nginx --replica=4

- generate a deployment yaml file (output the definition into yml -o yaml) and save it, don't create it (--dry-run=client)
kubectl create deployment --image=nginx nginx --dry-run=client -o yaml > nginx-deployment.yaml
- make changes to yaml file and create it
kubectl create -f nginx-deployment.yaml

## Namespaces
- Q7 --> What DNS name should the Blue application use to access the database db-service in the dev namespace? 
service.dev.svc.cluter.local

## Services
- Q4: Create a service redis-service to expose the redis application within the cluster on port 6379
kubectl expose pod redis --port=6379 --name redis-service

- Q6: Create a new pod called custom-nginx using the nginx image and run it on container port 8080.
kubectl run custom-nginx --image=nginx --port=8080

- Q9:Create a pod called httpd using the image httpd:alpine in the default namespace. Next, create a service of type ClusterIP by the same name (httpd). The target port for the service should be 80.
*kubectl run httpd --image=httpd:alpine --port=80 --expose*
service/httpd created
pod/httpd created

## Imperative Commands
- create a service named redis-service of type ClusterIP to expose pod redis on port 6379
kubectl expose pod redis --port=6379 --name redis-service --dry-run=client -o yaml  ?? it will use pod's labels as selectors automatically

- create a service named nginx of type NodePort to expose pod nginx's port 80 on port 30080 on the nodes
kubectl expose pod nginx --type=NodePort --port=80 --name=nginx-service --dry-run=client -o yaml 
--> add node port in the yaml file

# 3. Scheduling
## Mannual scheduling
- Q3 Why is the POD in a pending state?
kubectl get pods --namespace kube-system  --> to see the status of scheduler pod. 
We have removed the scheduler from this Kubernetes cluster. As a result, as it stands, the pod will remain in a pending state forever.
![sc2](../../images/sc2.PNG)  add nodeName to manual assign node

