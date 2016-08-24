##What is it?

```

A Pet Set, in contrast, is a group of stateful pods that require a stronger notion of identity.

```

##Lets create a yaml file for this demo

```

$ vim petset.yaml

# A headless service to create DNS records
apiVersion: v1
kind: Service
metadata:
  name: nginx
  labels:
    app: nginx
spec:
  ports:
  - port: 80
    name: web
  # *.nginx.default.svc.cluster.local
  clusterIP: None
  selector:
    app: nginx
---
apiVersion: apps/v1alpha1
kind: PetSet
metadata:
  name: web
spec:
  serviceName: "nginx"
  replicas: 2
  template:
    metadata:
      labels:
        app: nginx
      annotations:
        pod.alpha.kubernetes.io/initialized: "true"
    spec:
      terminationGracePeriodSeconds: 0
      containers:
      - name: nginx
        image: gcr.io/google_containers/nginx-slim:0.8
        ports:
        - containerPort: 80
          name: web
        volumeMounts:
        - name: www
          mountPath: /usr/share/nginx/html
  volumeClaimTemplates:
  - metadata:
      name: www
      annotations:
        volume.alpha.kubernetes.io/storage-class: anything
    spec:
      accessModes: [ "ReadWriteOnce" ]
      resources:
        requests:
          storage: 1Gi

```

##Well lets creat the pod

```
$ kubectl create -f petset.yaml 
service "nginx" created
petset "web" created

```

##Get the status

```

$ kubectl get po -o wide
NAME                                READY     STATUS    RESTARTS   AGE       IP          NODE
busybox                             1/1       Running   47         1d        10.48.4.4   gke-kubernetes-try-out-default-pool-8a9aff9a-537a
nginx-deployment-1159050644-ir115   1/1       Running   0          2d        10.48.2.5   gke-kubernetes-try-out-default-pool-8a9aff9a-02e7
nginx-deployment-1159050644-m3mro   1/1       Running   0          2d        10.48.4.3   gke-kubernetes-try-out-default-pool-8a9aff9a-537a
nginx-deployment-1159050644-s5sqn   1/1       Running   0          2d        10.48.3.3   gke-kubernetes-try-out-default-pool-8a9aff9a-kg5t

```

##Get the volumes created by it

```
$ kubectl get pv
NAME                                       CAPACITY   ACCESSMODES   STATUS    CLAIM               REASON    AGE
pvc-20ebcf38-69d4-11e6-887a-42010af00014   1Gi        RWO           Bound     default/www-web-0             3m
pvc-20edf37a-69d4-11e6-887a-42010af00014   1Gi        RWO           Bound     default/www-web-1             3m

```

##Get the service status

```

$ kubectl get svc
NAME         CLUSTER-IP    EXTERNAL-IP   PORT(S)   AGE
kubernetes   10.51.240.1   <none>        443/TCP   2d
nginx        None          <none>        80/TCP    6m

```

## Oh wait! why you are not seeing the custer ip value,becasue it is a HEADLESS service,means without cluster 

##Lets get a stable hostname 

```
$ for i in 0 1; do kubectl exec web-$i -- sh -c 'hostname'; done


