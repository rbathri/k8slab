##Get the files

```
exec-liveness.yaml http-liveness.yaml 

From kubernetes source : /kubernetes/docs/user-guide/liveness


```
##Here are those two yaml files look like 


##This is exec-liveness.yaml
```
apiVersion: v1
kind: Pod
metadata:
  labels:
    test: liveness
  name: liveness-exec
spec:
  containers:
  - args:
    - /bin/sh
    - -c
    - echo ok > /tmp/health; sleep 10; rm -rf /tmp/health; sleep 600
    image: gcr.io/google_containers/busybox
    livenessProbe:
      exec:
        command:
        - cat
        - /tmp/health
      initialDelaySeconds: 15
      timeoutSeconds: 1
    name: liveness-exec
```

##And below is http-liveness.yaml
```
apiVersion: v1
kind: Pod
metadata:
  labels:
    test: liveness
  name: liveness-http
spec:
  containers:
  - args:
    - /server
    image: gcr.io/google_containers/liveness
    livenessProbe:
      httpGet:
        path: /healthz
        port: 8080
        httpHeaders:
          - name: X-Custom-Header
            value: Awesome
      initialDelaySeconds: 15
      timeoutSeconds: 1
    name: liveness-http

```

## Utilise those file to check the output.


```
$ kubectl create -f docs/user-guide/liveness/exec-liveness.yaml
$ kubectl create -f docs/user-guide/liveness/http-liveness.yaml

```

##Get the pod enlisting

```

$ kubectl get pods
NAME                                           READY     STATUS       RESTARTS   AGE
[...]
liveness-exec                                  1/1       Running      0          13s
liveness-http                                  1/1       Running      0          13s

```

##Check for restart count

```

$ kubectl get pods
NAME                                           READY     STATUS       RESTARTS   AGE
[...]
liveness-exec                                  1/1       Running      1          36s
liveness-http                                  1/1       Running      1          36s

```

##Described all 

```

$ kubectl describe pods liveness-exec
[...]
Sat, 27 Jun 2015 13:43:03 +0200    Sat, 27 Jun 2015 13:44:34 +0200    4    {kubelet kubernetes-node-6fbi}    spec.containers{liveness}    unhealthy  Liveness probe failed: cat: can't open '/tmp/health': No such file or directory
Sat, 27 Jun 2015 13:44:44 +0200    Sat, 27 Jun 2015 13:44:44 +0200    1    {kubelet kubernetes-node-6fbi}    spec.containers{liveness}    killing    Killing with docker id 65b52d62c635
Sat, 27 Jun 2015 13:44:44 +0200    Sat, 27 Jun 2015 13:44:44 +0200    1    {kubelet kubernetes-node-6fbi}    spec.containers{liveness}    created    Created with docker id ed6bb004ee10
Sat, 27 Jun 2015 13:44:44 +0200    Sat, 27 Jun 2015 13:44:44 +0200    1    {kubelet kubernetes-node-6fbi}    spec.containers{liveness}    started    Started with docker id ed6bb004ee10

```

##Watch out all the above commands in action

[![asciicast](https://asciinema.org/a/91g02511t85xdm8npq4n918rr.png)](https://asciinema.org/a/91g02511t85xdm8npq4n918rr)
