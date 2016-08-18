##Get the yaml file from kunernetes source folder 

```
$ cd /kubernetes/docs/user-guide/job.yaml

```

##And how it look like 

```
apiVersion: batch/v1
kind: Job
metadata:
  name: pi
spec:
  template:
    metadata:
      name: pi
    spec:
      containers:
      - name: pi
        image: perl
        command: ["perl",  "-Mbignum=bpi", "-wle", "print bpi(2000)"]
      restartPolicy: Never

```

##Well, lets create the job by kubectl 

```
$ kubectl create -f job.yaml
job "pi" created

```

##Describe it aka check the metadata about the created job

```
$ kubectl describe jobs/pi
Name:		pi
Namespace:	default
Image(s):	perl
Selector:       controller-uid=b1db589a-2c8d-11e6-b324-0209dc45a495
Parallelism:	1
Completions:	1
Start Time:     Tue, 07 Jun 2016 10:56:16 +0200
Labels:         controller-uid=b1db589a-2c8d-11e6-b324-0209dc45a495,job-name=pi
Pods Statuses:	0 Running / 1 Succeeded / 0 Failed
No volumes.
Events:
  FirstSeen	LastSeen	Count	From			SubobjectPath	Type		Reason			Message
  ---------	--------	-----	----			-------------	--------	------			-------
  1m		1m		1	{job-controller }			Normal		SuccessfulCreate	Created pod: pi-dtn4q

```

##List out pod relevent to the pods regarding jobs

```
$ pods=$(kubectl get pods --selector=job-name=pi --output=jsonpath={.items..metadata.name})
echo $pods
pi-aiw0a

```

##Gets the log of it

```
$ kubectl logs $pods

```

##Scale it to a greater number

```
$ kubectl scale --replicas=5 jobs/pi

```

##Lets delete it

```

$ kubectl delete jobs/pi

```

## Watch out all the commands in action!!

[![asciicast](https://asciinema.org/a/cdvi8zy1jmvjg6vk838240mcd.png)](https://asciinema.org/a/cdvi8zy1jmvjg6vk838240mcd)
