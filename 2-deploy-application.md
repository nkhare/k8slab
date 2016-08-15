```
$ wget https://raw.githubusercontent.com/kubernetes/kubernetes.github.io/release-1.3/docs/user-guide/run-my-nginx.yaml
$ kubectl create -f ./run-my-nginx.yaml
$ kubectl get deployment
$ kubectl get pods
```

## Get labels
```
$ kubectl get pods -L run
```

## Get Seletctots
```
$  kubectl get deployment/my-nginx -o template --template="{{.spec.selector}}"
```

```
$ kubectl delete deployment/my-nginx
```
