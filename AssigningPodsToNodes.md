##Heads up

```
Generally this is unnecessary, as the SCHEDULER will take care of things for you.

```

##But,situation compelles you to do it ..heck

```
kubectl label nodes <node-name> <label-key>=<label-value>

```

##Get the nodes 

```

$ kubectl get nodes
NAME                                        STATUS    AGE
gke-kubernetes-default-pool-b3d6d977-77wi   Ready     20h
gke-kubernetes-default-pool-b3d6d977-8ils   Ready     20h
gke-kubernetes-default-pool-b3d6d977-nxc7   Ready     20h
gke-kubernetes-default-pool-b3d6d977-pf58   Ready     20h
gke-kubernetes-default-pool-b3d6d977-yc49   Ready     20h

```

##Label it

```
Important notes:

label keys must be in the form of DNS labels,meaning that they are not allowed to contain any upper-case letters

$ kubectl label node gke-kubernetes-default-pool-b3d6d977-77wi disktype=ssd
node "gke-kubernetes-default-pool-b3d6d977-77wi" labeled

```

##Check it 

```

$ kubectl describe node gke-kubernetes-default-pool-b3d6d977-77wi | grep disktype
                        disktype=ssd

```

## Need to see inside metadata of node :)

```
$ kubectl describe node gke-kubernetes-default-pool-b3d6d977-77wi
Name:                   gke-kubernetes-default-pool-b3d6d977-77wi
Labels:                 beta.kubernetes.io/arch=amd64
                        beta.kubernetes.io/instance-type=n1-standard-1
                        beta.kubernetes.io/os=linux
                        cloud.google.com/gke-nodepool=default-pool
                        disktype=ssd
                        failure-domain.beta.kubernetes.io/region=asia-east1
                        failure-domain.beta.kubernetes.io/zone=asia-east1-a
                        kubernetes.io/hostname=gke-kubernetes-default-pool-b3d6d977-77wi
Taints:                 <none>
CreationTimestamp:      Thu, 18 Aug 2016 19:51:17 +0530
Phase:  
Conditions:
  Type                  Status  LastHeartbeatTime                       LastTransitionTime                      Reason                          Message
  ----                  ------  -----------------                       ------------------                      ------                          -------
  NetworkUnavailable    False   Mon, 01 Jan 0001 00:00:00 +0000         Thu, 18 Aug 2016 19:51:49 +0530         RouteCreated                    RouteController created a route
  OutOfDisk             False   Fri, 19 Aug 2016 16:17:43 +0530         Thu, 18 Aug 2016 19:51:17 +0530         KubeletHasSufficientDisk        kubelet has sufficient disk space available
  MemoryPressure        False   Fri, 19 Aug 2016 16:17:43 +0530         Thu, 18 Aug 2016 19:51:17 +0530         KubeletHasSufficientMemory      kubelet has sufficient memory available
  Ready                 True    Fri, 19 Aug 2016 16:17:43 +0530         Thu, 18 Aug 2016 19:52:05 +0530         KubeletReady                    kubelet is posting ready status. WARNING: CPU hardcapping unsupported
Addresses:              10.140.0.2,104.199.202.235
Capacity:
 alpha.kubernetes.io/nvidia-gpu:        0
 cpu:                                   1
 memory:                                3801016Ki
 pods:                                  110
Allocatable:
 alpha.kubernetes.io/nvidia-gpu:        0
 cpu:                                   1
 memory:                                3801016Ki
 pods:                                  110
System Info:
 Machine ID:
 System UUID:                   CE85821D-620F-29B8-FED2-73420F65EBA2
 Boot ID:                       339af5fd-174c-44ab-8f6a-33aee66b0f2f
 Kernel Version:                3.16.0-4-amd64
 OS Image:                      Debian GNU/Linux 7 (wheezy)
 Operating System:              linux
 Architecture:                  amd64
 Container Runtime Version:     docker://1.11.2
 Kubelet Version:               v1.3.4
 Kube-Proxy Version:            v1.3.4
PodCIDR:                        10.0.3.0/24
ExternalID:                     7804076465281596697
Non-terminated Pods:            (4 in total)
  Namespace                     Name                                                                    CPU Requests    CPU Limits      Memory Requests Memory Limits
  ---------                     ----                                                                    ------------    ----------      --------------- -------------
  default                       nginx-rolling-update-040c09f66f84bdcc560bc34a0ea939ed-0iccb             100m (10%)      0 (0%)          0 (0%)          0 (0%)
  default                       nginx-rolling-update-040c09f66f84bdcc560bc34a0ea939ed-y5fps             100m (10%)      0 (0%)          0 (0%)          0 (0%)
  kube-system                   fluentd-cloud-logging-gke-kubernetes-default-pool-b3d6d977-77wi         100m (10%)      0 (0%)          200Mi (5%)      200Mi (5%)
  kube-system                   kube-proxy-gke-kubernetes-default-pool-b3d6d977-77wi                    100m (10%)      0 (0%)          0 (0%)          0 (0%)
Allocated resources:
  (Total limits may be over 100 percent, i.e., overcommitted. More info: http://releases.k8s.io/HEAD/docs/user-guide/compute-resources.md)
  CPU Requests  CPU Limits      Memory Requests Memory Limits
  ------------  ----------      --------------- -------------
  400m (40%)    0 (0%)          200Mi (5%)      200Mi (5%)
No events.

```

##Well,alternate approch through yaml file, how??

```
apiVersion: v1
kind: Pod
metadata:
  name: httpd
  labels:
    env: test
spec:
  containers:
  - name: httpd
    image: httpd
    imagePullPolicy: IfNotPresent
  nodeSelector:
    disktype: ssd

```

##And create the pod 

```
$ kubectl create -f httpd.yaml 
pod "httpd" created


```

##Inspect it 

```
$ kubectl get pod -o wide | grep httpd
httpd                                                         1/1       Running             0          2m        10.0.3.7   gke-kubernetes-default-pool-b3d6d977-77wi

```

## Get the lebel

```
$ kubectl describe nodes gke-kubernetes-default-pool-b3d6d977-77wi
Name:                   gke-kubernetes-default-pool-b3d6d977-77wi
Labels:                 beta.kubernetes.io/arch=amd64
                        beta.kubernetes.io/instance-type=n1-standard-1
                        beta.kubernetes.io/os=linux
                        cloud.google.com/gke-nodepool=default-pool
                        disktype=ssd
                        failure-domain.beta.kubernetes.io/region=asia-east1
                        failure-domain.beta.kubernetes.io/zone=asia-east1-a
                        kubernetes.io/hostname=gke-kubernetes-default-pool-b3d6d977-77wi
Taints:                 <none>
CreationTimestamp:      Thu, 18 Aug 2016 19:51:17 +0530
Phase:  
Conditions:
  Type                  Status  LastHeartbeatTime                       LastTransitionTime                      Reason                          Message
  ----                  ------  -----------------                       ------------------                      ------                          -------
  NetworkUnavailable    False   Mon, 01 Jan 0001 00:00:00 +0000         Thu, 18 Aug 2016 19:51:49 +0530         RouteCreated                    RouteController created a route
  OutOfDisk             False   Fri, 19 Aug 2016 16:43:39 +0530         Thu, 18 Aug 2016 19:51:17 +0530         KubeletHasSufficientDisk        kubelet has sufficient disk space available
  MemoryPressure        False   Fri, 19 Aug 2016 16:43:39 +0530         Thu, 18 Aug 2016 19:51:17 +0530         KubeletHasSufficientMemory      kubelet has sufficient memory available
  Ready                 True    Fri, 19 Aug 2016 16:43:39 +0530         Thu, 18 Aug 2016 19:52:05 +0530         KubeletReady                    kubelet is posting ready status. WARNING: CPU hardcapping unsupported
Addresses:              10.140.0.2,104.199.202.235
Capacity:
 alpha.kubernetes.io/nvidia-gpu:        0
 cpu:                                   1
 memory:                                3801016Ki
 pods:                                  110
Allocatable:
 alpha.kubernetes.io/nvidia-gpu:        0
 cpu:                                   1
 memory:                                3801016Ki
 pods:                                  110
System Info:
 Machine ID:
 System UUID:                   CE85821D-620F-29B8-FED2-73420F65EBA2
 Boot ID:                       339af5fd-174c-44ab-8f6a-33aee66b0f2f
 Kernel Version:                3.16.0-4-amd64
 OS Image:                      Debian GNU/Linux 7 (wheezy)
 Operating System:              linux
 Architecture:                  amd64
 Container Runtime Version:     docker://1.11.2
 Kubelet Version:               v1.3.4
 Kube-Proxy Version:            v1.3.4
PodCIDR:                        10.0.3.0/24
ExternalID:                     7804076465281596697
Non-terminated Pods:            (7 in total)
  Namespace                     Name                                                                    CPU Requests    CPU Limits      Memory Requests Memory Limits
  ---------                     ----                                                                    ------------    ----------      --------------- -------------
  default                       alpine                                                                  100m (10%)      0 (0%)          0 (0%)          0 (0%)
  default                       busybox                                                                 100m (10%)      0 (0%)          0 (0%)          0 (0%)
  default                       httpd                                                                   100m (10%)      0 (0%)          0 (0%)          0 (0%)
  default                       nginx-rolling-update-040c09f66f84bdcc560bc34a0ea939ed-0iccb             100m (10%)      0 (0%)          0 (0%)          0 (0%)
  default                       nginx-rolling-update-040c09f66f84bdcc560bc34a0ea939ed-y5fps             100m (10%)      0 (0%)          0 (0%)          0 (0%)
  kube-system                   fluentd-cloud-logging-gke-kubernetes-default-pool-b3d6d977-77wi         100m (10%)      0 (0%)          200Mi (5%)      200Mi (5%)
  kube-system                   kube-proxy-gke-kubernetes-default-pool-b3d6d977-77wi                    100m (10%)      0 (0%)          0 (0%)          0 (0%)
Allocated resources:
  (Total limits may be over 100 percent, i.e., overcommitted. More info: http://releases.k8s.io/HEAD/docs/user-guide/compute-resources.md)
  CPU Requests  CPU Limits      Memory Requests Memory Limits
  ------------  ----------      --------------- -------------
  700m (70%)    0 (0%)          200Mi (5%)      200Mi (5%)
No events.
```

##Ah, could be filter out like this

```
$ kubectl describe nodes gke-kubernetes-default-pool-b3d6d977-77wi | grep disktype
                        disktype=ssd

```

##Built in node labels

```
kubernetes.io/hostname

failure-domain.beta.kubernetes.io/zone

failure-domain.beta.kubernetes.io/region

beta.kubernetes.io/instance-type

```

## Node afinity is a Alpha featue ...lets not discuss it right now..may be sometime ..



