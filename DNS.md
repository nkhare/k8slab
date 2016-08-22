##Create a yaml file 

```
apiVersion: v1
kind: Pod
metadata:
  name: busybox
  namespace: default
spec:
  hostname: busybox-1
  subdomain: default
  containers:
  - image: busybox
    command:
      - sleep
      - "3600"
    name: busybox

```

## Create the pod

```

$ kubectl create -f dns.yaml
pod "busybox" created

```

##Get the status 
```
$ kubectl get pods busybox
NAME      READY     STATUS    RESTARTS   AGE
busybox   1/1       Running   0          3m

```

##Validate DNS work

```
$ kubectl exec busybox -- nslookup kubernetes.default
Server:    10.51.240.10
Address 1: 10.51.240.10 kube-dns.kube-system.svc.cluster.local

Name:      kubernetes.default
Address 1: 10.51.240.1 kubernetes.default.svc.cluster.local

```

##Voila! it works
