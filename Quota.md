##Get into the kubernetes source dir 

```
$ cd /kubernetes/docs/admin/resourcequota

$ ls 

```

##Create a namespace

```
$ kubectl create -f docs/admin/resourcequota/namespace.yaml

```

##Apply an object count quota tothe namespace

```

$ kubectl create -f docs/admin/resourcequota/object-counts.yaml --namespace=quota-example
resourcequota "object-counts" created

```

##Find out what's in it

```
$ kubectl describe quota object-counts --namespace=quota-exampl

```

##Apply a compute resource quota to the namespace

```

$ kubectl create -f docs/admin/resourcequota/compute-resources.yaml --namespace=quota-example
resourcequota "compute-resources" created

```

##Describe it

```
$ kubectl describe quota compute-resources --namespace=quota-example

Name:                  compute-resources
Namespace:             quota-example
Resource               Used Hard
--------               ---- ----
limits.cpu             0    2
limits.memory          0    2Gi
pods                   0    4
requests.cpu           0    1
requests.memory        0    1Gi

```

##Default resource quota and limit

```

$ kubectl run nginx --image=nginx --replicas=1 --namespace=quota-example
deployment "nginx" created

```

##Look at the pods

```

$ kubectl get pods --namespace=quota-example

```
##Describe it one more time as no output come from previous command

```

$ kubectl describe deployment nginx --namespace=quota-example
Name:                   nginx
Namespace:              quota-example
CreationTimestamp:      Mon, 06 Jun 2016 16:11:37 -0400
Labels:                 run=nginx
Selector:               run=nginx
Replicas:               0 updated | 1 total | 0 available | 1 unavailable
StrategyType:           RollingUpdate
MinReadySeconds:        0
RollingUpdateStrategy:  1 max unavailable, 1 max surge
OldReplicaSets:         <none>
NewReplicaSet:          nginx-3137573019 (0/1 replicas created)

```

##Lets dig in into replica sets

```

$ kubectl describe rs nginx-3137573019 --namespace=quota-example
Name:                   nginx-3137573019
Namespace:              quota-example
Image(s):               nginx
Selector:               pod-template-hash=3137573019,run=nginx
Labels:                 pod-template-hash=3137573019
                        run=nginx
Replicas:               0 current / 1 desired
Pods Status:            0 Running / 0 Waiting / 0 Succeeded / 0 Failed
No volumes.
Events:
  FirstSeen	LastSeen	Count From                    SubobjectPath Type      Reason        Message
  ---------	--------  ----- ----                    -------------	--------  ------        -------
  4m        7s        11    {replicaset-controller }              Warning   FailedCreate  Error creating: pods "nginx-3137573019-" is forbidden: Failed quota: compute-resources: must specify limits.cpu,limits.memory,requests.cpu,requests.memory

```

##Set default values for cpu and memory limit quota

```

$ kubectl create -f docs/admin/resourcequota/limits.yaml --namespace=quota-example
limitrange "limits" created
$ kubectl describe limits limits --namespace=quota-example
Name:           limits
Namespace:      quota-example
Type      Resource  Min  Max  Default Request   Default Limit   Max Limit/Request Ratio
----      --------  ---  ---  ---------------   -------------   -----------------------
Container memory    -    -    256Mi             512Mi           -
Container cpu       -    -    100m              200m            -

```

##Alternatively you can do the same on cli

```

$ kubectl run nginx \
  --image=nginx \
  --replicas=1 \
  --requests=cpu=100m,memory=256Mi \
  --limits=cpu=200m,memory=512Mi \
  --namespace=quota-example

```

##Lets check

```
$ kubectl get pods --namespace=quota-example
NAME                     READY     STATUS    RESTARTS   AGE
nginx-3137573019-fvrig   1/1       Running   0          6m

```

##Lets print the quota usage 

```

$ kubectl describe quota --namespace=quota-example
Name:           compute-resources
Namespace:      quota-example
Resource        Used    Hard
--------        ----    ----
limits.cpu      200m    2
limits.memory   512Mi   2Gi
pods            1       4
requests.cpu    100m    1
requests.memory 256Mi   1Gi


Name:                 object-counts
Namespace:            quota-example
Resource              Used    Hard
--------              ----    ----
persistentvolumeclaims 0      2
services.loadbalancers 0      2
services.nodeports     0      0

```

##Advance quota scope

``` 
$ kubectl create namespace quota-scopes
namespace "quota-scopes" created

```

##best effort

```

$ kubectl create -f docs/admin/resourcequota/best-effort.yaml --namespace=quota-scopes
resourcequota "best-effort" created

```

##No best effort

```

$ kubectl create -f docs/admin/resourcequota/not-best-effort.yaml --namespace=quota-scopes
resourcequota "not-best-effort" created

```

##Describe it 

```

$ kubectl describe quota --namespace=quota-scopes
Name:       best-effort
Namespace:  quota-scopes
Scopes:     BestEffort
 * Matches all pods that have best effort quality of service.
Resource    Used	Hard
--------    ----  ----
pods        0     10


Name:             not-best-effort
Namespace:        quota-scopes
Scopes:           NotBestEffort
 * Matches all pods that do not have best effort quality of service.
Resource          Used  Hard
--------          ----  ----
limits.cpu        0     2
limits.memory     0     2Gi
pods              0     4
requests.cpu      0     1
requests.memory   0     1Gi

```

##Apply the above efforts

```

$ kubectl run best-effort-nginx --image=nginx --replicas=8 --namespace=quota-scopes
deployment "best-effort-nginx" created

```

##One more for other one

```
$ kubectl run not-best-effort-nginx \
  --image=nginx \
  --replicas=2 \
  --requests=cpu=100m,memory=256Mi \
  --limits=cpu=200m,memory=512Mi \
  --namespace=quota-scopes
deployment "not-best-effort-nginx" created

```

##Get those pods

```

$ kubectl get pods --namespace=quota-scopes
NAME                                     READY     STATUS    RESTARTS   AGE
best-effort-nginx-3488455095-2qb41       1/1       Running   0          51s
best-effort-nginx-3488455095-3go7n       1/1       Running   0          51s
best-effort-nginx-3488455095-9o2xg       1/1       Running   0          51s
best-effort-nginx-3488455095-eyg40       1/1       Running   0          51s
best-effort-nginx-3488455095-gcs3v       1/1       Running   0          51s
best-effort-nginx-3488455095-rq8p1       1/1       Running   0          51s
best-effort-nginx-3488455095-udhhd       1/1       Running   0          51s
best-effort-nginx-3488455095-zmk12       1/1       Running   0          51s
not-best-effort-nginx-2204666826-7sl61   1/1       Running   0          23s
not-best-effort-nginx-2204666826-ke746   1/1       Running   0          23s

```

##Current quota useage

```

$ kubectl describe quota --namespace=quota-scopes
Name:            best-effort
Namespace:       quota-scopes
Scopes:          BestEffort
 * Matches all pods that have best effort quality of service.
Resource         Used  Hard
--------         ----  ----
pods             8     10


Name:               not-best-effort
Namespace:          quota-scopes
Scopes:             NotBestEffort
 * Matches all pods that do not have best effort quality of service.
Resource            Used  Hard
--------            ----  ----
limits.cpu          400m  2
limits.memory       1Gi   2Gi
pods                2     4
requests.cpu        200m  1
requests.memory     512Mi 1Gi

```
##Watch all the commands in action 

[![asciicast](https://asciinema.org/a/6dy88z53ootsoe1oyjmm80j5e.png)](https://asciinema.org/a/6dy88z53ootsoe1oyjmm80j5e)

