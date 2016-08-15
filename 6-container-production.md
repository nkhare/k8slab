## Persistent Storage

```
$ wget https://raw.githubusercontent.com/kubernetes/kubernetes.github.io/release-1.3/docs/user-guide/redis-deployment.yaml
```
http://kubernetes.io/docs/user-guide/volumes


## Distributing Secrets
Kubernetes provides a mechanism, called secrets, that facilitates delivery of sensitive credentials to applications. A Secret is a simple resource containing a map of data. For instance, you can create a simple secret with a username and password as follows: 

```
$ kubectl create secret generic mysecret --from-literal=username="admin",password="1234
```

Which is same as following:-
```
apiVersion: v1
kind: Secret
metadata:
  name: mysecret
type: Opaque
data:
  username: YWRtaW4=
  password: MTIzNA==
```

Use them in the Pod
```
$ wget https://raw.githubusercontent.com/kubernetes/kubernetes.github.io/release-1.3/docs/user-guide/redis-secret-deployment.yaml
$ kubectl create -f redis-secret-deployment.yaml 
```

http://kubernetes.io/docs/user-guide/secrets

[![asciicast](https://asciinema.org/a/9d7za224ei7mgvlhkr88dcmah.png)](https://asciinema.org/a/9d7za224ei7mgvlhkr88dcmah)

## Authenticating with private registry

```
$ kubectl create secret docker-registry nkhareregistrykey --docker-username=nkharre --docker-password=●●●●●●●●●●● --docker-email=neependra.khare@gmail.com
```

Create a pod with following  file
```
apiVersion: v1
kind: Pod
metadata:
  name: foo
spec:
  containers:
    - name: foo
      image: janedoe/awesomeapp:v1
  imagePullSecrets:
    - name: myregistrykey
```


## Helper Container
```
apiVersion: extensions/v1beta1
kind: Deployment
metadata:
  name: my-nginx
spec:
  template:
    metadata:
      labels:
        app: nginx
    spec:
      volumes:
      - name: www-data
        emptyDir: {}
      containers:
      - name: nginx
        image: nginx
        # This container reads from the www-data volume
        volumeMounts:
        - mountPath: /srv/www
          name: www-data
          readOnly: true
      - name: git-monitor
        image: myrepo/git-monitor
        env:
        - name: GIT_REPO
          value: http://github.com/some/repo.git
        # This container writes to the www-data volume
        volumeMounts:
        - mountPath: /data
          name: www-data
```


## Resource Management

```
$ kubectl describe limitrange limits
```

```
$ wget https://raw.githubusercontent.com/kubernetes/kubernetes.github.io/release-1.3/docs/user-guide/redis-resource-deployment.yaml
```


## Liveness and Readiness probe
Many applications running for long periods of time eventually transition to broken states, and cannot recover except by restarting them. Kubernetes provides liveness probes to detect and remedy such situations.

Other times, applications are only temporarily unable to serve, and will recover on their own. Typically in such cases you’d prefer not to kill the application, but don’t want to send it requests, either, since the application won’t respond correctly or at all. A common such scenario is loading large data or configuration files during application startup. Kubernetes provides readiness probes to detect and mitigate such situations. Readiness probes are configured similarly to liveness probes, just using the readinessProbe field. 

```
$ wget https://raw.githubusercontent.com/kubernetes/kubernetes.github.io/release-1.3/docs/user-guide/nginx-probe-deployment.yaml
```


## Handling Initialization

```
$ wget https://raw.githubusercontent.com/kubernetes/kubernetes.github.io/release-1.3/docs/user-guide/nginx-init-containers.yaml
$ kubectl -f nginx-init-containers.yaml
```

## Lifecycle hooks and termination notice
```
$ wget https://raw.githubusercontent.com/kubernetes/kubernetes.github.io/release-1.3/docs/user-guide/nginx-lifecycle-deployment.yaml
```

## Terminanting Message

```
$ wget  https://raw.githubusercontent.com/kubernetes/kubernetes.github.io/release-1.3/docs/user-guide/pod-w-message.yaml
``` 


```
$ kubectl create -f ./pod-w-message.yaml
$ sleep 70
$ kubectl get pods/pod-w-message -o go-template="{{range .status.containerStatuses}}{{.lastState.terminated.message}}{{end}}"
$ kubectl get pods/pod-w-message -o go-template="{{range .status.containerStatuses}}{{.lastState.terminated.exitCode}}{{end}}"
0
```
