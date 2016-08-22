##Get into the kubernetes source dir

```
$ cd /kubernetes/docs/user-guides/

```

##Create a file for this section name counter-pod.yaml

```
$ vim counter-pod.yaml
apiVersion: v1
kind: Pod
metadata:
  name: counter
spec:
  containers:
  - name: count
    image: ubuntu:14.04
    args: [bash, -c, 
           'for ((i = 0; ; i++)); do echo "$i: $(date)"; sleep 1; done']

```

## Create the pod

```
$ kubectl create -f counter-pod.yaml 
pod "counter" created

```

##Get the status of the new pod

```

$ kubectl get pod | grep counter
counter                                                       1/1       Running             0          1m

```

##Get the logs of the created pod

```
$ kubectl logs counter
0: Sat Aug 20 08:14:44 UTC 2016
1: Sat Aug 20 08:14:45 UTC 2016
2: Sat Aug 20 08:14:46 UTC 2016
3: Sat Aug 20 08:14:47 UTC 2016
4: Sat Aug 20 08:14:48 UTC 2016
5: Sat Aug 20 08:14:49 UTC 2016
6: Sat Aug 20 08:14:50 UTC 2016
7: Sat Aug 20 08:14:51 UTC 2016
8: Sat Aug 20 08:14:52 UTC 2016
9: Sat Aug 20 08:14:53 UTC 2016
10: Sat Aug 20 08:14:54 UTC 2016
11: Sat Aug 20 08:14:55 UTC 2016
12: Sat Aug 20 08:14:56 UTC 2016
13: Sat Aug 20 08:14:57 UTC 2016
14: Sat Aug 20 08:14:58 UTC 2016
15: Sat Aug 20 08:14:59 UTC 2016
16: Sat Aug 20 08:15:00 UTC 2016
17: Sat Aug 20 08:15:01 UTC 2016
18: Sat Aug 20 08:15:02 UTC 2016
19: Sat Aug 20 08:15:03 UTC 2016
20: Sat Aug 20 08:15:04 UTC 2016
21: Sat Aug 20 08:15:05 UTC 2016
22: Sat Aug 20 08:15:06 UTC 2016
23: Sat Aug 20 08:15:07 UTC 2016
24: Sat Aug 20 08:15:08 UTC 2016
25: Sat Aug 20 08:15:09 UTC 2016
26: Sat Aug 20 08:15:10 UTC 2016
27: Sat Aug 20 08:15:11 UTC 2016
28: Sat Aug 20 08:15:12 UTC 2016
29: Sat Aug 20 08:15:13 UTC 2016
30: Sat Aug 20 08:15:14 UTC 2016
31: Sat Aug 20 08:15:15 UTC 2016
32: Sat Aug 20 08:15:16 UTC 2016
33: Sat Aug 20 08:15:17 UTC 2016
34: Sat Aug 20 08:15:18 UTC 2016
35: Sat Aug 20 08:15:19 UTC 2016
36: Sat Aug 20 08:15:20 UTC 2016
37: Sat Aug 20 08:15:21 UTC 2016
38: Sat Aug 20 08:15:22 UTC 2016
39: Sat Aug 20 08:15:23 UTC 2016
40: Sat Aug 20 08:15:24 UTC 2016
41: Sat Aug 20 08:15:25 UTC 2016
42: Sat Aug 20 08:15:26 UTC 2016
43: Sat Aug 20 08:15:27 UTC 2016
44: Sat Aug 20 08:15:28 UTC 2016
45: Sat Aug 20 08:15:29 UTC 2016
46: Sat Aug 20 08:15:30 UTC 2016
47: Sat Aug 20 08:15:31 UTC 2016
48: Sat Aug 20 08:15:32 UTC 2016
49: Sat Aug 20 08:15:33 UTC 2016
50: Sat Aug 20 08:15:34 UTC 2016
51: Sat Aug 20 08:15:35 UTC 2016
52: Sat Aug 20 08:15:36 UTC 2016
53: Sat Aug 20 08:15:37 UTC 2016
54: Sat Aug 20 08:15:38 UTC 2016
55: Sat Aug 20 08:15:39 UTC 2016
56: Sat Aug 20 08:15:40 UTC 2016
57: Sat Aug 20 08:15:41 UTC 2016
58: Sat Aug 20 08:15:42 UTC 2016
59: Sat Aug 20 08:15:43 UTC 2016
60: Sat Aug 20 08:15:44 UTC 2016
61: Sat Aug 20 08:15:45 UTC 2016
62: Sat Aug 20 08:15:46 UTC 2016
63: Sat Aug 20 08:15:47 UTC 2016
64: Sat Aug 20 08:15:48 UTC 2016
65: Sat Aug 20 08:15:49 UTC 2016
66: Sat Aug 20 08:15:50 UTC 2016
67: Sat Aug 20 08:15:51 UTC 2016
68: Sat Aug 20 08:15:52 UTC 2016
69: Sat Aug 20 08:15:53 UTC 2016
70: Sat Aug 20 08:15:54 UTC 2016
71: Sat Aug 20 08:15:55 UTC 2016
72: Sat Aug 20 08:15:56 UTC 2016
73: Sat Aug 20 08:15:57 UTC 2016
74: Sat Aug 20 08:15:58 UTC 2016
75: Sat Aug 20 08:15:59 UTC 2016
76: Sat Aug 20 08:16:00 UTC 2016
77: Sat Aug 20 08:16:01 UTC 2016
78: Sat Aug 20 08:16:02 UTC 2016
79: Sat Aug 20 08:16:03 UTC 2016
80: Sat Aug 20 08:16:04 UTC 2016
81: Sat Aug 20 08:16:05 UTC 2016
82: Sat Aug 20 08:16:06 UTC 2016
83: Sat Aug 20 08:16:07 UTC 2016
84: Sat Aug 20 08:16:08 UTC 2016
85: Sat Aug 20 08:16:09 UTC 2016
86: Sat Aug 20 08:16:10 UTC 2016
87: Sat Aug 20 08:16:11 UTC 2016
88: Sat Aug 20 08:16:12 UTC 2016
89: Sat Aug 20 08:16:13 UTC 2016
90: Sat Aug 20 08:16:14 UTC 2016
91: Sat Aug 20 08:16:15 UTC 2016
92: Sat Aug 20 08:16:16 UTC 2016
93: Sat Aug 20 08:16:17 UTC 2016
94: Sat Aug 20 08:16:18 UTC 2016
95: Sat Aug 20 08:16:19 UTC 2016
96: Sat Aug 20 08:16:20 UTC 2016
97: Sat Aug 20 08:16:21 UTC 2016
98: Sat Aug 20 08:16:22 UTC 2016
99: Sat Aug 20 08:16:23 UTC 2016
100: Sat Aug 20 08:16:24 UTC 2016
101: Sat Aug 20 08:16:25 UTC 2016
102: Sat Aug 20 08:16:26 UTC 2016
103: Sat Aug 20 08:16:27 UTC 2016
104: Sat Aug 20 08:16:28 UTC 2016
105: Sat Aug 20 08:16:29 UTC 2016
106: Sat Aug 20 08:16:30 UTC 2016
107: Sat Aug 20 08:16:31 UTC 2016
108: Sat Aug 20 08:16:32 UTC 2016
109: Sat Aug 20 08:16:33 UTC 2016
110: Sat Aug 20 08:16:34 UTC 2016
111: Sat Aug 20 08:16:35 UTC 2016
112: Sat Aug 20 08:16:36 UTC 2016
113: Sat Aug 20 08:16:37 UTC 2016
114: Sat Aug 20 08:16:38 UTC 2016
115: Sat Aug 20 08:16:39 UTC 2016
116: Sat Aug 20 08:16:40 UTC 2016
117: Sat Aug 20 08:16:41 UTC 2016
118: Sat Aug 20 08:16:42 UTC 2016
119: Sat Aug 20 08:16:43 UTC 2016
120: Sat Aug 20 08:16:44 UTC 2016
121: Sat Aug 20 08:16:45 UTC 2016
122: Sat Aug 20 08:16:46 UTC 2016
123: Sat Aug 20 08:16:47 UTC 2016
124: Sat Aug 20 08:16:48 UTC 2016
125: Sat Aug 20 08:16:49 UTC 2016
126: Sat Aug 20 08:16:50 UTC 2016
127: Sat Aug 20 08:16:51 UTC 2016
128: Sat Aug 20 08:16:52 UTC 2016
129: Sat Aug 20 08:16:53 UTC 2016
130: Sat Aug 20 08:16:54 UTC 2016
131: Sat Aug 20 08:16:55 UTC 2016
132: Sat Aug 20 08:16:56 UTC 2016
133: Sat Aug 20 08:16:57 UTC 2016
134: Sat Aug 20 08:16:58 UTC 2016
135: Sat Aug 20 08:16:59 UTC 2016
136: Sat Aug 20 08:17:00 UTC 2016
137: Sat Aug 20 08:17:01 UTC 2016
138: Sat Aug 20 08:17:02 UTC 2016
139: Sat Aug 20 08:17:03 UTC 2016
140: Sat Aug 20 08:17:04 UTC 2016
141: Sat Aug 20 08:17:05 UTC 2016
142: Sat Aug 20 08:17:06 UTC 2016
143: Sat Aug 20 08:17:07 UTC 2016
144: Sat Aug 20 08:17:08 UTC 2016
145: Sat Aug 20 08:17:09 UTC 2016
146: Sat Aug 20 08:17:10 UTC 2016
147: Sat Aug 20 08:17:11 UTC 2016
148: Sat Aug 20 08:17:12 UTC 2016
149: Sat Aug 20 08:17:13 UTC 2016
150: Sat Aug 20 08:17:14 UTC 2016
```

##Lets see the metadata about this pod
```
$ kubectl describe pod counter
Name:           counter
Namespace:      default
Node:           gke-kubernetes-default-pool-b3d6d977-yc49/10.140.0.3
Start Time:     Sat, 20 Aug 2016 13:44:27 +0530
Labels:         <none>
Status:         Running
IP:             10.0.2.4
Controllers:    <none>
Containers:
  count:
    Container ID:       docker://fb1ff17ab4d68e3456131c8f56b2a41539e38ba529b57e4308cc28219c967523
    Image:              ubuntu:14.04
    Image ID:           docker://sha256:ff60113363279ed0eb02aadd3149368b414dcf9baa4d3ce3c1fc8b6f4a03875e
    Port:
    Args:
      bash
      -c
      for ((i = 0; ; i++)); do echo "$i: $(date)"; sleep 1; done
    Requests:
      cpu:                      100m
    State:                      Running
      Started:                  Sat, 20 Aug 2016 13:44:44 +0530
    Ready:                      True
    Restart Count:              0
    Environment Variables:      <none>
Conditions:
  Type          Status
  Initialized   True 
  Ready         True 
  PodScheduled  True 
Volumes:
  default-token-fchtp:
    Type:       Secret (a volume populated by a Secret)
    SecretName: default-token-fchtp
QoS Tier:       Burstable
Events:
  FirstSeen     LastSeen        Count   From                                                    SubobjectPath           Type            Reason          Message
  ---------     --------        -----   ----                                                    -------------           --------        ------          -------
  5m            5m              1       {default-scheduler }                                                            Normal          Scheduled       Successfully assigned counter to gke-kubernetes-default-pool-b3d6d977-yc49
  5m            5m              1       {kubelet gke-kubernetes-default-pool-b3d6d977-yc49}     spec.containers{count}  Normal          Pulling         pulling image "ubuntu:14.04"
  4m            4m              1       {kubelet gke-kubernetes-default-pool-b3d6d977-yc49}     spec.containers{count}  Normal          Pulled          Successfully pulled image "ubuntu:14.04"
  4m            4m              1       {kubelet gke-kubernetes-default-pool-b3d6d977-yc49}     spec.containers{count}  Normal          Created         Created container with docker id fb1ff17ab4d6
  4m            4m              1       {kubelet gke-kubernetes-default-pool-b3d6d977-yc49}     spec.containers{count}  Normal          Started         Started container with docker id fb1ff17ab4d6

```

##Lets get into it

```

$ kubectl exec -it couter bash
root@counter:/# ps aux
USER       PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root         1  0.0  0.0  17984  2912 ?        Ss   08:14   0:00 bash -c for ((i
root      1118  0.0  0.0  17952  2820 ?        Ss   08:20   0:00 bash
root      1835  0.0  0.0  18144  3068 ?        Ss   08:24   0:00 bash
root      1882  0.0  0.0   4348   808 ?        S    08:25   0:00 sleep 1
root      1883  0.0  0.0  15568  2220 ?        R+   08:25   0:00 ps aux
```

##Lets delete the pod

```
$ kubectl delete pod counter
pod "counter" deleted

```

##Integrate with GCE with fluentd

##Get a gce-fluentd yaml file

```
$ cd /kubernetes/cluster/addons/gci 
$ ls
fluentd-gcp.yaml  README.md

```

##How it look like inside the file!

```
# This config should be kept as similar as possible to the one at
# cluster/saltbase/salt/fluentd-gcp/fluentd-gcp.yaml
apiVersion: v1
kind: Pod
metadata:
- name: fluentd-cloud-logging
    image: gcr.io/google_containers/fluentd-gcp:1.21
    command:
      - '/bin/sh'
      - '-c'
      # This is pretty hacky, but ruby relies on libsystemd's native code, and
      # the ubuntu:16.04 libsystemd doesn't play nice with the journal on GCI
      # hosts. Work around the problem by copying in the host's libsystemd.
      - 'rm /lib/x86_64-linux-gnu/libsystemd* && cp /host/lib/libsystemd* /lib/x86_64-linux-gnu/ && /usr/sbin/google-fluentd -q -c /etc/google-fluentd/google-fluentd-journal.conf'
    resources:
      limits:
        memory: 200Mi
      requests:
        # Any change here should be accompanied by a proportional change in CPU
        # requests of other per-node add-ons (e.g. kube-proxy).
        cpu: 80m
        memory: 200Mi
    volumeMounts:
    - name: varlog
      mountPath: /var/log
    - name: varlibdockercontainers
      mountPath: /var/lib/docker/containers
      readOnly: true
    - name: journaldir
      mountPath: /var/log/journal
    - name: libsystemddir
      mountPath: /host/lib
  terminationGracePeriodSeconds: 30
  volumes:
  - name: varlog
    hostPath:
      path: /var/log
  - name: varlibdockercontainers
    hostPath:
      path: /var/lib/docker/containers
  - name: journaldir
    hostPath:
      path: /var/log/journal
  - name: libsystemddir
    hostPath:
      path: /usr/lib64

```

## We need to head over to the gce and check the logging section

