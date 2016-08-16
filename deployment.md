##Get the yaml file
```
$ wget https://raw.githubusercontent.com/kubernetes/kubernetes.github.io/release-1.3/docs/user-guide/nginx-deployment.yaml

```

##create the nginx deployment
```
$kubectl create -f docs/user-guide/nginx-deployment.yaml --record
```

##Get the deployment

```
$ kubectl get deployments
```
##Get the replica set

```
$ kubectl get rs
```
## Get the labels of the pods
```
$ kubectl get pods --show-labels
```
##Get the specific rollout status 
```
$ kubectl rollout status deployment/nginx-deployment
```
##Get the deployment 

```
$ kubectl get deployments
```
## Update the pod image
```
$ kubectl set image deployment/nginx-deployment nginx=nginx:1.9.1
```
##Or alternatively you can do the same by editing the yaml file directly
```
$ kubectl edit deployment/nginx-deployment
```
##Get the rollout status one more time
```
$ kubectl rollout status deployment/nginx-deployment
```
##Get the deployment count
```
$ kubectl get deployments
```
##Get the replica sets count
```
$ kubectl get rs
```
##Get the pod counts
```
$ kubectl get pods
```
##Get the metadata about the deployment
```
$ kubectl describe deployments

```
##Get the status of the deployment
```
$ kubectl rollout status deployments nginx-deployment

```
##Get the replica sets
```

$ kubectl get rs

```
##Get the pos counts
```
$ kubectl get pods

```
##Get the metadata about deployment 
```

$ kubectl describe deployment

```
## Show us the history of the rollout
```

$ kubectl rollout history deployment/nginx-deployment

```
## Specific version of the rollout history
```

$ kubectl rollout history deployment/nginx-deployment --revision=2

```
## Revert back with the deployment
```

$ kubectl rollout undo deployment/nginx-deployment

```
## To a specific number
```

$ kubectl rollout undo deployment/nginx-deployment --to-revision=2

```
##Get the deployment count
```

$ kubectl get deployment

```
##Pause the deployment,if you do it again
```

$ kubectl rollout pause deployment/nginx-deployment

```
##Get the replica set count
```

$ kubectl get rs 

```
##Get us status of rollout of a specific deployment
```

$ kubectl rollout status deployment/nginx-deployment

```
##Resume the halted/paused deployment
```

$ kubectl rollout resume deployment/nginx-deployment

```
##Get us the status one more time ,please
```

$ kubectl rollout status deployment/nginx-deployment

```
##Get us the replica sets 
```

$ kubectl get rs 

```
##Watch out!!

[![asciicast](https://asciinema.org/a/6c5y9h3znombrn832v65tkxs2.png)](https://asciinema.org/a/6c5y9h3znombrn832v65tkxs2)
