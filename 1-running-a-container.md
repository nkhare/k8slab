
```
$ kubectl run my-nginx --image=nginx --replicas=2 --port=80
$ kubectl get pods
$ kubectl get deployments
$ kubectl expose deployment my-nginx --port=80 --type=LoadBalancer
$ kubectl get services
$ kubectl delete deployment my-nginx
```
