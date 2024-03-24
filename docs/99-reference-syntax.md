## Troubleshoot
Masi error?
coba jalanin
```bash
sudo chown -R $USER $HOME/.minikube; chmod -R u+wrx $HOME/.minikube
```
lalu jalankan ini
```bash
minikube start --force-systemd=true
```



## Detail Nodes
```bash
kubectl get nodes -o wide
```

## Get Pods list
```bash
kubectl get pods
```

## Create pods
```bash
kubectl run nginx --image=nginx
```

## Show Detail Pods 
```bash
kubectl describe pod newpods-l4fzk
```

## Information Details
| NAME          | READY   | STATUS          | RESTARTS      | AGE |
|---------------|---------|-----------------|---------------|-----|
| nginx         | 1/1     | Running         | 0             | 22m |
| newpods-wx6xx | 1/1     | Running         | 1 (4m37s ago) | 21m |
| newpods-5jk6g | 1/1     | Running         | 1 (4m37s ago) | 21m |
| newpods-l4fzk | 1/1     | Running         | 1 (4m37s ago) | 21m |
| webapp        | 1/2     | ImagePullBackOff| 0             | 17m |

READY
total running container pod / total container on pod

## Delete Pods
```bash
kubectl delete pod webapp
```

# create pod using yaml
We use `kubectl run` command with `--dry-run=client -o` yaml option to create a manifest file :-
```bash
kubectl run redis --image=redis123 --dry-run=client -o yaml > redis-definition.yaml
```

After that, using `kubectl create -f` command to create a resource from the manifest file :-

```bash
kubectl create -f redis-definition.yaml 
```

Verify the work by running kubectl get command :-
```bash
kubectl get pods
```

## Edit

```bash
kubectl edit pod redis
```

re-apply
```bash
kubectl apply -f redis-definition.yaml 
```

## create replication
```bash
kubectl create -f replicaset.yaml
```
## upscale replication
```bash
kubectl scale rs new-replica-set --replicas=5
```

## update replica set
```bash
kubectl edit replicaset new-replica-set
```

## get detail replicaset
```bash
kubectl describe rs new-replica-set
```

# Deployment

## create deployment
```bash
kubectl create -f deployment-definition.yaml
```

## get all deploymnet
```bash
kubectl get deployments
```

## get replication
```bash
kubectl get replicaset
```

## get all information
```bash
kubectl get all
```

## update deployment only image version
```bash
kubectl set image deployment-definition.yaml nginx-container:1.9.1
```

## rollback deployment
```bash
kubectl rollback undo deployment/myapp-development
```

## rollback status deployment
```bash
kubectl rollback status deployment/myapp-development
```

## rollback status deployment
```bash
kubectl history status deployment/myapp-development
```

# Services

## Get All services
```bash
kubectl get service
```

## Get All services
```bash
kubectl get service
```

## Get all svc
```bash
kubectl get svc
```

## Get detail svc
```bash
kubectl describe svc kubernetes
```