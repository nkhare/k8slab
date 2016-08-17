## Requests vs Limits
- request [optional]: the amount of resources being requested, or that were requested and have been allocated. Scheduler algorithms will use these quantities to test feasibility (whether a pod will fit onto a node). If a container (or pod) tries to use more resources than its request, any associated SLOs are voided — e.g., the program it is running may be throttled (compressible resource types), or the attempt may be denied. If request is omitted for a container, it defaults to limit if that is explicitly specified, otherwise to an implementation-defined value; this will always be 0 for a user-defined resource type. If request is omitted for a pod, it defaults to the sum of the (explicit or implicit) request values for the containers it encloses.

- limit [optional]: an upper bound or cap on the maximum amount of resources that will be made available to a container or pod; if a container or pod uses more resources than its limit, it may be terminated. The limit defaults to "unbounded"; in practice, this probably means the capacity of an enclosing container, pod, or node, but may result in non-deterministic behavior, especially for memory.


## CPU and Memory resource 
### CPU
- specified in unit of cores
- One cpu, in Kubernetes, is equivalent to:
-- 1 AWS vCPU
-- 1 GCP Core
-- 1 Azure vCore
-- 1 Hyperthread on a bare-metal Intel processor with Hyperthreading


|Unit|	Amount|
|----|--------|
|1000m|	1000 milli-cores == 100% CPU core|
|500m|	500 milli-cores == 50% CPU core|
|250m|	250 milli-cores == 25% CPU core|
|100m|	100 milli-cores == 10% CPU core|


### Memory
- Limits and requests for memory are measured in bytes. Memory can be expressed a plain integer or as fixed-point integers with one of these SI suffixes (E, P, T, G, M, K) or their power-of-two equivalents (Ei, Pi, Ti, Gi, Mi, Ki). For example, the following represent roughly the same value: 128974848, 129e6, 129M , 123Mi.


```
apiVersion: v1
kind: Pod
metadata:
  name: frontend
spec:
  containers:
  - name: db
    image: mysql
    resources:
      requests:
        memory: "64Mi"
        cpu: "250m"
      limits:
        memory: "128Mi"
        cpu: "500m"
  - name: wp
    image: wordpress
    resources:
      requests:
        memory: "64Mi"
        cpu: "250m"
      limits:
        memory: "128Mi"
        cpu: "500m"
```

## Create the namespace 

```
$ kubectl create namespace limit-example
$ kubectl get namespaces
```

## Apply limits to namespace

```
$ kubectl create -f docs/admin/limitrange/limits.yaml --namespace=limit-example
```

```
apiVersion: v1
kind: LimitRange
metadata:
  name: mylimits
spec:
  limits:
  - max:
      cpu: "2"
      memory: 1Gi
    min:
      cpu: 200m
      memory: 6Mi
    type: Pod
  - default:
      cpu: 300m
      memory: 200Mi
    defaultRequest:
      cpu: 200m
      memory: 100Mi
    max:
      cpu: "2"
      memory: 1Gi
    min:
      cpu: 100m
      memory: 3Mi
    type: Container
```

### Look at the namespace limits

```
$ kubectl describe limits mylimits --namespace=limit-example

```


## Enforcing limits at point of creation

```
$ kubectl run nginx --image=nginx --replicas=1 --namespace=limit-example
$ kubectl get pods --namespace=limit-example
$ kubectl get pods nginx-2040093540-s8vzu --namespace=limit-example -o yaml | grep resources -C 8
$ kubectl create -f docs/admin/limitrange/invalid-pod.yaml --namespace=limit-example
$ kubectl create -f docs/admin/limitrange/valid-pod.yaml --namespace=limit-example
pod "valid-pod" created
```

## Clean up 
$ kubectl delete namespace limit-example

