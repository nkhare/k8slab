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

[![asciicast](https://asciinema.org/a/7t4qds1jmd00qex9oiggavam4.png)](https://asciinema.org/a/7t4qds1jmd00qex9oiggavam4)

<script type="text/javascript" src="https://asciinema.org/a/7t4qds1jmd00qex9oiggavam4.js" id="asciicast-7t4qds1jmd00qex9oiggavam4" async></script>
