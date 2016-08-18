##Get into the kubernetes source dir and make the change

```
$ cd /kubernetes/docs/user-guide/

```

##Update the existing image and pod has been crated with this file

```
apiVersion: v1
kind: ReplicationController
metadata:
  name: nginx-rolling-update
spec:
  replicas: 5
  template:
    metadata:
      labels:
        app: nginx-rolling-update
    spec:
      containers:
      - name: nginx-rolling-update
        image: nginx:1.7.9
        ports:
        - containerPort: 325

```

## Update it 
```
$ kubectl rolling-update nginx-rolling-update --image=nginx:1.9.1
Created my-nginx-ccba8fbd8cc8160970f63f9a2696fc46

```

##Get the deployment label too

```
$ kubectl get pods -l app=nginx-rolling-update -L deployment
NAME                                              READY     STATUS    RESTARTS   AGE       DEPLOYMENT
my-nginx-ccba8fbd8cc8160970f63f9a2696fc46-k156z   1/1       Running   0          1m        ccba8fbd8cc8160970f63f9a2696fc46
my-nginx-ccba8fbd8cc8160970f63f9a2696fc46-v95yh   1/1       Running   0          35s       ccba8fbd8cc8160970f63f9a2696fc46
my-nginx-divi2                                    1/1       Running   0          2h        2d1d7a8f682934a254002b56404b813e
my-nginx-o0ef1                                    1/1       Running   0          2h        2d1d7a8f682934a254002b56404b813e
my-nginx-q6all                                    1/1       Running   0          8m        2d1d7a8f682934a254002b56404b813e

```

##Rollback the stuff

```

$ kubectl rolling-update nginx-rolling-update --rollback
Setting "my-nginx" replicas to 1
Continuing update with existing controller my-nginx.
Scaling up nginx from 1 to 1, scaling down my-nginx-ccba8fbd8cc8160970f63f9a2696fc46 from 1 to 0 (keep 1 pods available, don't exceed 2 pods)
Scaling my-nginx-ccba8fbd8cc8160970f63f9a2696fc46 down to 0
Update succeeded. Deleting my-nginx-ccba8fbd8cc8160970f63f9a2696fc46
replicationcontroller "my-nginx" rolling updated

```

##Watch out all the commands in action!!

[![asciicast](https://asciinema.org/a/6p62gcyxf06jnx7lrvk7kni4y.png)](https://asciinema.org/a/6p62gcyxf06jnx7lrvk7kni4y)
