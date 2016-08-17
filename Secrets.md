##Creating your own secret

```
$ echo -n "kubernetes" > ./username.txt

$ echo -n "cloudyuga" > ./password.txt

```

##Create by kubectl 

```

$ kubectl create secret generic db-user-pass --from-file=./username.txt --from-file=./password.txt
secret "db-user-pass" created

```

##Get the secret -display

```

$ kubectl get secret 

NAME                  TYPE                                  DATA      AGE
db-user-pass          Opaque                                2         51s

$ kubectl describe secrets/db-user-pass
Name:		db-user-pass
Namespace:	default
Labels:		<none>
Annotations:	<none>

Type:	Opaque

Data
====
password.txt:	13 bytes
username.txt:	6 bytes

```

##Creating secret manually

```

$ echo -n "admin" | base64
YWRtaW4=
$ echo -n "1f2d1e2e67df" | base64
MWYyZDFlMmU2N2Rm

```

##Create the secret file by kubectl once more

```
$ kubectl create -f ./secret.yaml
secret "mysecret" created

```

##Decoding a secret

```
$ kubectl get secret mysecret -o yaml
apiVersion: v1
data:
  password: MWYyZDFlMmU2N2Rm
  username: YWRtaW4=
kind: Secret
metadata:
  creationTimestamp: 2016-01-22T18:41:56Z
  name: mysecret
  namespace: default
  resourceVersion: "164619"
  selfLink: /api/v1/namespaces/default/secrets/mysecret
  uid: cfee02d6-c137-11e5-8d73-42010af00002
type: Opaque

```

##Decoding password field!!!!!!!!!

```

$ echo "MWYyZDFlMmU2N2Rm" | base64 -d
1f2d1e2e67df

```

##Using secret

```

Mount a secret in volume 

{
 "apiVersion": "v1",
 "kind": "Pod",
  "metadata": {
    "name": "mypod",
    "namespace": "myns"
  },
  "spec": {
    "containers": [{
      "name": "mypod",
      "image": "redis",
      "volumeMounts": [{
        "name": "foo",
        "mountPath": "/etc/foo",
        "readOnly": true
      }]
    }],
    "volumes": [{
      "name": "foo",
      "secret": {
        "secretName": "mysecret"
      }
    }]
  }
}

```

##using secret in env variable

```

apiVersion: v1
kind: Pod
metadata:
  name: secret-env-pod
spec:
  containers:
    - name: mycontainer
      image: redis
      env:
        - name: SECRET_USERNAME
          valueFrom:
            secretKeyRef:
              name: mysecret
              key: username
        - name: SECRET_PASSWORD
          valueFrom:
            secretKeyRef:
              name: mysecret
              key: password
  restartPolicy: Never

```

##Use cases: pod with ssh keys

```
$ kubectl create secret generic my-secret --from-file=ssh-privatekey=/path/to/.ssh/id_rsa --from-file=ssh-publickey=/path/to/.ssh/id_rsa.pub

```

##And the yaml file looks something like this

```

{
  "kind": "Pod",
  "apiVersion": "v1",
  "metadata": {
    "name": "secret-test-pod",
    "labels": {
      "name": "secret-test"
    }
  },
  "spec": {
    "volumes": [
      {
        "name": "secret-volume",
        "secret": {
          "secretName": "ssh-key-secret"
        }
      }
    ],
    "containers": [
      {
        "name": "ssh-test-container",
        "image": "mySshImage",
        "volumeMounts": [
          {
            "name": "secret-volume",
            "readOnly": true,
            "mountPath": "/etc/secret-volume"
          }
        ]
      }
    ]

```

##How to access it inside container

```
/etc/secret-volume/id-rsa.pub
/etc/secret-volume/id-rsa

```

