---
title: "Kubernetes Application Lifecycle Management"
date: 2024-06-19T13:23:47+09:00
featuredImage: "/images/featured-image/lifecycle.jpeg"
tags: ["Kubernetes"]
categories: ["Development"]
draft: true
---

## Rolloing Updates and Roll Back

### Q. Let us try that. Upgrade the application by setting the image on the deployment to kodekloud/webapp-color:v2

Do not delete and re-create the deployment. Only set the new image name for the existing deployment.

Deployment Name: frontend

Deployment Image: kodekloud/webapp-color:v2

```bash
controlplane ~ ➜  kubectl edit deployment frontend

apiVersion: apps/v1
kind: Deployment
metadata:
  annotations:
    deployment.kubernetes.io/revision: "1"
  creationTimestamp: "2024-06-19T04:55:08Z"
  generation: 1
  name: frontend
  namespace: default
  resourceVersion: "897"
  uid: 4b75042c-8a82-42dd-86f9-c4cd41f4a8e8
spec:
  minReadySeconds: 20
  progressDeadlineSeconds: 600
  replicas: 4
  revisionHistoryLimit: 10
  selector:
    matchLabels:
      name: webapp
  strategy:
    rollingUpdate:
      maxSurge: 25%
      maxUnavailable: 25%
    type: RollingUpdate
  template:
    metadata:
      creationTimestamp: null
      labels:
        name: webapp
    spec:
      containers:
      - image: kodekloud/webapp-color:v2
        imagePullPolicy: IfNotPresent
        name: simple-webapp
        ports:
        - containerPort: 8080
          protocol: TCP
        resources: {}
        terminationMessagePath: /dev/termination-log
        terminationMessagePolicy: File
      dnsPolicy: ClusterFirst
      restartPolicy: Always
      schedulerName: default-scheduler
      securityContext: {}
      terminationGracePeriodSeconds: 30
status:
:wq

deployment.apps/frontend edited
```

### Q. Change the deployment strategy to `Recreate`.

Delete and re-create the deployment if neccessary. Only update the strategy type of the existing deployment.

Deployment Name: frontend

Deployment Image: kodekloud/webapp-color:v2

Strategy: Recreate

```bash
controlplane ~ ✖ kubectl edit deployment frontend

---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: frontend
  namespace: default
spec:
  replicas: 4
  selector:
    matchLabels:
      name: webapp
  strategy:
    type: Recreate
  template:
    metadata:
      labels:
        name: webapp
    spec:
      containers:
      - image: kodekloud/webapp-color:v2
        name: simple-webapp
        ports:
        - containerPort: 8080
          protocol: TCP
:wq

deployment.apps/frontend edited
```

## Commands and Arguments

EX.

pod-definition.yaml

```yaml
apiVersion: v1
kind: Pod
metadata: 
  name: ubuntu-sleeper-pod
spec:
  containers:
    - name: ubuntu-sleeper
      image: ubuntu-sleeper
      command: ["sleep2.0"] # -> ENTRYPOINT
      args: ["10"] # -> CMD
```

Docker Image
```Dockerfile
FROM ubuntu

ENTRYPOINT ["sleep"]

CMD ["5"]
```

{{<admonition info>}}
`ENTRYPOINT`와 `CMD`는 Dockerfile에서 컨테이너가 시작될 때 실행할 명령을 지정하는 데 사용되는 두 가지 중요한 지시어입니다. 이 두 가지는 종종 혼동되기 쉬우므로, 각 지시어의 역할과 차이점을 이해하는 것이 중요합니다.

- ENTRYPOINT

    - ENTRYPOINT는 Docker 컨테이너가 시작될 때 실행되는 명령을 지정합니다. 
    
    - ENTRYPOINT로 설정된 명령은 항상 실행되며, 일반적으로 변경되지 않습니다. 

- CMD
    - CMD는 Command에 전달되는 default parameter 입니다.

    - CMD는 컨테이너가 시작될 때 실행할 기본 명령을 지정합니다. 
    
    - CMD는 ENTRYPOINT가 설정되지 않았을 때 실행되며, ENTRYPOINT와 함께 사용되면 ENTRYPOINT에 전달할 기본 인수를 
    제공할 수도 있습니다. 
    
    - CMD는 ENTRYPOINT와 달리, 컨테이너를 실행할 때 명령을 재정의할 수 있습니다.
{{</admonition>}}

### Q. What is the command used to run the pod `ubuntu-sleeper`?

```bash
controlplane ~ ➜  kubectl describe pod ubuntu-sleeper
Name:             ubuntu-sleeper
Namespace:        default
Priority:         0
Service Account:  default
Node:             controlplane/192.9.202.3
Start Time:       Wed, 19 Jun 2024 05:59:09 +0000
Labels:           <none>
Annotations:      <none>
Status:           Running
IP:               10.42.0.9
IPs:
  IP:  10.42.0.9
Containers:
  ubuntu:
    Container ID:  containerd://31ffda30d5faa144528105f8c94028eafc7df17943b2eb901303e9593a5783a0
    Image:         ubuntu
    Image ID:      docker.io/library/ubuntu@sha256:2e863c44b718727c860746568e1d54afd13b2fa71b160f5cd9058fc436217b30
    Port:          <none>
    Host Port:     <none>
    Command:
      sleep
      4800
```

> A. sleep 4800

### Q. Create a pod with the ubuntu image to run a container to sleep for 5000 seconds. Modify the file `ubuntu-sleeper-2.yaml`.

Pod Name: ubuntu-sleeper-2

Command: sleep 5000

```bash
controlplane ~ ➜  vim ubuntu-sleeper-2.yaml

---
apiVersion: v1 
kind: Pod 
metadata:
  name: ubuntu-sleeper-2 
spec:
  containers:
  - name: ubuntu
    image: ubuntu
    command:
      - "sleep"
      - "5000"
:wq

controlplane ~ ✖ kubectl create -f ubuntu-sleeper-2.yaml 
pod/ubuntu-sleeper-2 created
```

### Q. Inspect the file `Dockerfile` given at `/root/webapp-color` directory. What command is run at container startup?

```bash
controlplane ~/webapp-color ➜  cat Dockerfile
FROM python:3.6-alpine

RUN pip install flask

COPY . /opt/

EXPOSE 8080

WORKDIR /opt

ENTRYPOINT ["python", "app.py"]
```

> A. python app.py

### Q. Inspect the file `Dockerfile2` given at `/root/webapp-color` directory. What command is run at container startup?

```bash
controlplane ~/webapp-color ➜  cat Dockerfile2
FROM python:3.6-alpine

RUN pip install flask

COPY . /opt/

EXPOSE 8080

WORKDIR /opt

ENTRYPOINT ["python", "app.py"]

CMD ["--color", "red"]
```

> A. python app.py --color red

### Q. Inspect the two files under directory `webapp-color-2`. What command is run at container startup?

```bash
controlplane ~/webapp-color-2 ➜  cat Dockerfile
FROM python:3.6-alpine

RUN pip install flask

COPY . /opt/

EXPOSE 8080

WORKDIR /opt

ENTRYPOINT ["python", "app.py"]

CMD ["--color", "red"]

controlplane ~/webapp-color-2 ➜  cat webapp-color-pod.yaml 
apiVersion: v1 
kind: Pod 
metadata:
  name: webapp-green
  labels:
      name: webapp-green 
spec:
  containers:
  - name: simple-webapp
    image: kodekloud/webapp-color
    command: ["--color","green"]
```

> A. --color green

### Q. Inspect the two files under directory webapp-color-3. What command is run at container startup?

```bash
controlplane ~/webapp-color-3 ➜  cat Dockerfile
FROM python:3.6-alpine

RUN pip install flask

COPY . /opt/

EXPOSE 8080

WORKDIR /opt

ENTRYPOINT ["python", "app.py"]

CMD ["--color", "red"]

controlplane ~/webapp-color-3 ➜  cat webapp-color-pod-2.yaml 
apiVersion: v1 
kind: Pod 
metadata:
  name: webapp-green
  labels:
      name: webapp-green 
spec:
  containers:
  - name: simple-webapp
    image: kodekloud/webapp-color
    command: ["python", "app.py"]
    args: ["--color", "pink"]
```

> A. python app.py --color pink

### Q. Create a pod with the given specifications. By default it displays a `blue` background. Set the given command line arguments to change it to `green`.

Pod Name: webapp-green

Image: kodekloud/webapp-color

Command line arguments: --color=green

```bash
controlplane ~ ➜  vim webapp-green-pod.yaml

---
apiVersion: v1 
kind: Pod 
metadata:
  name: webapp-green
  labels:
      name: webapp-green 
spec:
  containers:
  - name: simple-webapp
    image: kodekloud/webapp-color
    args: ["--color", "green"]
:wq

controlplane ~ ➜  kubectl create -f webapp-green-pod.yaml 
pod/webapp-green created
```