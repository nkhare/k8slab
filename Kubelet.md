##Kubelet,what it is??

```

The kubelet is the primary “node agent” that runs on each node. The kubelet works in terms of a PodSpec. A PodSpec is a YAML or JSON object that describes a pod. The kubelet takes a set of PodSpecs that are provided through various mechanisms (primarily through the apiserver) and ensures that the containers described in those PodSpecs are running and healthy.
Other than from an PodSpec from the apiserver, there are three ways that a container manifest can be provided to the Kubelet.

File: Path passed as a flag on the command line. This file is rechecked every 20 seconds (configurable with a flag).

HTTP endpoint: HTTP endpoint passed as a parameter on the command line. This endpoint is checked every 20 seconds (also configurable with a flag).

HTTP server: The kubelet can also listen for HTTP and respond to a simple API (underspec’d currently) to submit a new manifest.

```

## And the options are to run it 

```
$ kubelet ## this is the main binary

```

## Accompanying options with the binary

```
Please refer back to this documentaion 

http://kubernetes.io/docs/admin/kubelet/

```




