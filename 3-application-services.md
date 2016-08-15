```
$ wget  https://raw.githubusercontent.com/kubernetes/kubernetes.github.io/release-1.3/docs/user-guide/run-my-nginx.yaml
$ kubectl create -f ./run-my-nginx.yaml
$ kubectl get pods -l run=my-nginx -o wide
$ kubectl get pods -l run=my-nginx -o yaml | grep podIP
```

## Creating a service
```
$ kubectl expose deployment/my-nginx
```

OR

```
$ wget https://raw.githubusercontent.com/kubernetes/kubernetes.github.io/release-1.3/docs/user-guide/nginx-svc.yaml
$ kubectl create -f nginx-svc.yaml 
```

```
$ kubectl get svc my-nginx
```


## Accessing the serivce
### Environment Varibales
When a Pod is run on a Node, the kubelet adds a set of environment variables for each active Service.

```
$ kubectl exec my-nginx-3800858182-jr4a2 -- printenv | grep SERVICE
$ kubectl scale deployment my-nginx --replicas=0; kubectl scale deployment my-nginx --replicas=2;
$ kubectl exec my-nginx-3800858182-e9ihh -- printenv | grep SERVICE

```


### DNS
```
$ kubectl get services kube-dns --namespace=kube-system
$ kubectl run curl --image=radial/busyboxplus:curl -i --tty
nslookup my-nginx:
```


[![asciicast](https://asciinema.org/a/f2rnlwtd2ph2cnch3b7up3s57.png)](https://asciinema.org/a/f2rnlwtd2ph2cnch3b7up3s57)
