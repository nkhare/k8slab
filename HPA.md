##Autoscaling based on CPU usage.

##Create the pod for this demo

```
$ kubectl run php-apache --image=gcr.io/google_containers/hpa-example --requests=cpu=200m --expose --port=80
service "php-apache" created
deployment "php-apache" created

```

##Get the deployment status

```

$ kubect desctibe deployment php-apache

```

##Get the service status 

```

$ kubectl get svc php-apache

```

##Autoscale the existing stuff

```

$ kubectl autoscale deployment php-apache --cpu-percent=50 --min=1 --max=10
deployment "php-apache" autoscaled

```

##Get the hpa status

```

$ kubectl get hpa
NAME         REFERENCE                     TARGET    CURRENT   MINPODS   MAXPODS   AGE
php-apache   Deployment/php-apache/scale   50%       0%        1         10        18s

```

## Increase the load

```

$ kubectl run -i --tty load-generator --image=busybox /bin/sh
Hit enter for command prompt

```

##Once you are insider the container/pod , fire the below command
```
$ while true; do wget -q -O- http://php-apache.default.svc.cluster.local; done

```

## Again get the status

```

$ kubectl get hpa
NAME         REFERENCE                     TARGET    CURRENT   MINPODS   MAXPODS   AGE
php-apache   Deployment/php-apache/scale   50%       305%      1         10        3m

```

##Check deployment status too

```

$ kubectl get deployment php-apache
NAME         DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE
php-apache   7         7         7            7           19m

```

##Bring down the load

``` 
Once inside the contaiter,kill the loop we ran previously,look up the step above,kill it .

```

##Check the deployment status one more time 

```

$ kubectl get deployment php-apache
NAME         DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE
php-apache   1         1         1            1           27m

```

##Check the HPA status too

```

$ kubectl get hpa
NAME         REFERENCE                     TARGET    CURRENT   MINPODS   MAXPODS   AGE
php-apache   Deployment/php-apache/scale   50%       0%        1         10        11m

```


## Horizontal Pod Autoscaling

