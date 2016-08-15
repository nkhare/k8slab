## NodePort

```
$ kubectl get svc nginxsvc -o yaml | grep nodePort -C 
$ kubectl get nodes -o yaml | grep ExternalIP -C 1
$ curl https://<EXTERNAL-IP>:<NODE-PORT> -k
```

## LoadBalanacer
```
$ kubectl edit svc nginxsvc
$ curl https://<EXTERNAL-IP> -k
$ kubectl describe service nginxsvc
```
 
[![asciicast](https://asciinema.org/a/3hvafr5kqctwdxyls178c8hyp.png)](https://asciinema.org/a/3hvafr5kqctwdxyls178c8hyp)
