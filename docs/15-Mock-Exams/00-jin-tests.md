# PODs
create pod using yml
kubectl run redis --image=redis --dry-run=client -o yaml > redis.yaml
cat redis.yaml
kubectl create -f redis.yaml
vi redis.yaml
kubectl apply -f redis.yaml

# ReplicaSets
# Deployments

- generate pod manifest yaml file (-o yaml), don't create it (--dry-run)
kubectl run nginx --image=nginx --dry-run=client -o yaml

- create a deployment
kubectl create deployment --image=nginx nginx

- create a deployment yaml file (-o yaml), don't create it (--dry-run)
kubectl create deployment --image=nginx nginx --dry-run=client -o yaml

- generate a deployment yaml file (-o yaml) and save it, don't create it (--dry-run)
kubectl create deployment --image=nginx nginx --dry-run=client -o yaml > nginx-deployment.yaml
- make changes to yaml file and create it
kubectl create -f nginx-deployment.yaml