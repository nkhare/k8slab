- Namespaces provide a scope for names. Names of resources need to be unique within a namespace, but not across namespaces.
- Namespaces are a way to divide cluster resources between multiple uses (via resource quota).

## Viewing namespaces
```
$ kubectl get namespaces
```

## Setting the namespace for a request
```
$ kubectl --namespace=<insert-namespace-name-here> run nginx --image=nginx
$ kubectl --namespace=<insert-namespace-name-here> get pods
```

## Setting the namespace preference
```
$ export CONTEXT=$(kubectl config view | awk '/current-context/ {print $2}')
```

Then update the default namespace:
```
$ kubectl config set-context $CONTEXT --namespace=<insert-namespace-name-here>
$ kubectl config view | grep namespace:
```

## Namespaces and DNS
By default the entry is created in *<service-name>.<namespace-name>.svc.cluster.local* format

If you want to reach across namespaces, you need to use the fully qualified domain name (FQDN).




