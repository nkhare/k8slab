
- https://github.com/sjtud/kubernetes-beginners/blob/master/scheduler-for-beginners.md

## Trying out Bash Scheduler with
https://github.com/rothgar/bashScheduler

The Kubernetes scheduler’s task is to watch for pods having an empty PodSpec.NodeName value and attribute them a value to schedule the container somewhere in the cluster [32]. This is a difference compared to Swarm and Mesos as Kubernetes allows developers to bypass the scheduler by defining a PodSpec.NodeName when running the pod. The scheduler uses predicates [29] and priorities [30] to define on which nodes a pod should run. The default values of these parameters can be overridden using a new scheduler policy configuration [33].

### Predicates
Predicates are mandatory rules to schedule a new pod on the cluster. If no machine corresponds to the predicates required by the pod it will remain in pending state until a machine can satisfy them. The predicates available are:

- Predicate: node’s requirements
- PodFitPorts: needs to be able to host the pod without any port conflicts.
- PodFitsResources: has enough resources to host the pod.
- NoDiskConflict: has enough space to fit the pod and the volumes linked.
- MatchNodeSelector: match the selector query parameter defined in the pod’s description.
- HostName: has the name of the host parameter defined in the pod’s description.

### Priorities
If the scheduler finds multiple machines fitting the predicates the priorities will then be used to find what is the most suitable machine to run the pod. A priority is a key/value representing the name of the priority from the list of existing ones and its weight (importance of the priority). The priorities available are:

- Priority: node(s) considered as the best(s)
- LeastRequestedPriority: calculates the percentage of memory and CPU requested by the pods that are already on the node. The node with the minimum percentage is the best.
- BalancedResourceAllocation: nodes that have a similar memory and CPU usage.
- ServiceSpreadingPriority: prefers the nodes that have different pods using them.
- EqualPriority: only used for testing, give an equal priority to all the nodes in the cluster.


### References
- https://medium.com/@ArmandGrillet/comparison-of-container-schedulers-c427f4f7421#.21uw7nx6k
