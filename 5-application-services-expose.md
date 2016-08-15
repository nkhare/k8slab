## NodePort

```
$ kubectl get svc my-nginx -o yaml | grep nodePort -C 
$ kubectl get nodes -o yaml | grep ExternalIP -C 1
$ curl https://<EXTERNAL-IP>:<NODE-PORT> -k
```

## LoadBalanacer
```
$ kubectl edit svc my-nginx
$ curl https://<EXTERNAL-IP> -k
$ kubectl describe service my-nginx
```
 

