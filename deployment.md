```
$ wget https://raw.githubusercontent.com/kubernetes/kubernetes.github.io/release-1.3/docs/user-guide/nginx-deployment.yaml

```
```
$kubectl create -f docs/user-guide/nginx-deployment.yaml --record
```

```
$ kubectl get deployments
```

```
$ kubectl get deployments
```

```
$ kubectl get rs
```

```
$ kubectl get pods --show-labels
```

```
$ kubectl rollout status
```

```
$ kubectl rollout status deployment/nginx-deployment
```
```
$ kubectl get deployments
```
```
$ kubectl set image deployment/nginx-deployment nginx=nginx:1.9.1
```

```
$ kubectl edit deployment/nginx-deployment
```

```
$ kubectl rollout status deployment/nginx-deployment
```

```
$ kubectl get deployments
```

```
$ kubectl get rs
```

```
$ kubectl get pods
```

```
$ kubectl describe deployments

```

```
$ kubectl set image deployment/nginx-deployment nginx=nginx:1.9.1
```

```
$ kubectl rollout status deployments nginx-deployment

```

```

$ kubectl get rs

```

```
$ kubectl get pods

```

```

$ kubectl describe deployment

```

```

$ kubectl rollout history deployment/nginx-deployment

```

```

$ kubectl rollout history deployment/nginx-deployment --revision=2

```

```

$ kubectl rollout undo deployment/nginx-deployment

```

```

$ kubectl rollout undo deployment/nginx-deployment --to-revision=2

```

```

$ kubectl get deployment

```

```

$ kubectl set image deployment/nginx-deployment nginx=nginx:1.9.1; kubectl rollout pause deployment/nginx-deployment

```

```

$ kubectl get rs 

```

```

$ kubectl rollout status deployment/nginx-deployment

```

```

$ kubectl rollout resume deployment/nginx-deployment

```

```

$ kubectl rollout status deployment/nginx-deployment

```

```

$ kubectl get rs 

```


[![asciicast](https://asciinema.org/a/6c5y9h3znombrn832v65tkxs2.png)](https://asciinema.org/a/6c5y9h3znombrn832v65tkxs2)
