
```
$ kubectl run my-nginx --image=nginx --replicas=2 --port=80
$ kubectl get pods
$ kubectl get deployments
$ kubectl expose deployment my-nginx --port=80 --type=LoadBalancer
$ kubectl get services
$ kubectl delete deployment my-nginx
```

[![asciicast](https://asciinema.org/a/544tp8t2zs7tha2llxos6zogt.png)](https://asciinema.org/a/544tp8t2zs7tha2llxos6zogt)
