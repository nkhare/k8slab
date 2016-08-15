
```
$ cd https-nginx
$ make keys secret KEY=/tmp/nginx.key CERT=/tmp/nginx.crt SECRET=/tmp/secret.json
$ kubectl create -f /tmp/secret.json
$ kubectl get secrets
$ wget https://raw.githubusercontent.com/kubernetes/kubernetes/master/examples/https-nginx/nginx-app.yaml
$ kubectl delete rc,svc -l app=nginx; kubectl create -f ./nginx-app.yaml
$ kubectl get pods -o yaml | grep -i podip
$ curl -k https://10.244.3.5 
```

## Access the nginx from another pod
```
$ wget https://raw.githubusercontent.com/kubernetes/kubernetes.github.io/release-1.3/docs/user-guide/curlpod.yaml
$ kubectl create -f ./curlpod.yaml
$ kubectl get pods
$ kubectl exec curlpod -- curl https://my-nginx --cacert /etc/nginx/ssl/nginx.crt
```

