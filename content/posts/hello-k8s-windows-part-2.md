---
title: "Hello K8s in Windows - Part 2"
date: 2019-04-30T13:10:25-07:00
draft: false
---

This is the second in a multi-part series that will show how to easily get Kubernetes up and running in Windows and deploy a sample application. Almost everything also applies to Linux, MacOS, etc.

The source code for this example can be found at https://github.com/cleverswine/hello-k8s-win

These posts are besed on an intoductory presentation that I gave to my company.

### Deploy to a local Kubernetes (k8s) cluster

**Install the tools**

* Docker Desktop - enable Kubernetes after installing  (https://hub.docker.com/editions/community/docker-ce-desktop-windows)
* kubectl (https://kubernetes.io/docs/tasks/tools/install-kubectl/)

### Verify connectivity to the k8s cluster

Once your local k8s cluster is up and running, you should be able to interact with it using `kubectl`.

```powershell
PS> kubectl cluster-info
Kubernetes master is running at https://localhost:6445
KubeDNS is running at https://localhost:6445/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy

PS> kubectl get nodes
NAME                 STATUS    ROLES     AGE       VERSION
docker-for-desktop   Ready     master    2d        v1.10.3
```

### Clone the sample repository

```
git clone https://github.com/cleverswine/hello-k8s-win
```

### Build Docker images for the applications

Since the k8s cluster is running locally, we can just build the images locally and k8s will be able to find them. No need to publish to a remote repository.

```powershell
PS> docker build -t calculator:1.0.0 ./src/Calculator/
...
Successfully built 6b8146f7fdeb
Successfully tagged calculator:1.0.0

PS> docker build -t webui:1.0.0 ./src/WebUI/
...
Successfully built 6b8146f7fdeb
Successfully tagged webui:1.0.0
```

### Install rabbitmq and the applications

Use kubectl to do the installations

```
PS> kubectl apply -f deploy/rabbitmq.yaml
...
service/rabbitmq-headless created
service/rabbitmq created
statefulset.apps/rabbitmq created

PS> kubectl apply -f deploy/calculator.yaml
secret/calculator created
service/calculator created
deployment.apps/calculator created

PS> kubectl apply -f deploy/webui.yaml
secret/webui created
service/webui created
deployment.apps/webui created
```

### Verify the installation

Use `kubectl` to explore what was deployed. Try things like: `kubectl get all`, `kubectl get svc`, etc.

```powershell
PS> kubectl get pods
NAME                         READY     STATUS    RESTARTS   AGE
calculator-87dbdcdff-qh7f7   1/1       Running   0          16m
rabbitmq-0                   1/1       Running   0          16m
webui-675bfd674d-nthbz       1/1       Running   0          16m
```

Visit http://localhost:32500/ in the browser to see the WebUI in action.

**Scale out the calculator**

Manually scale the calculator service to two instances.

```powershell
kubectl scale --replicas=2 deployment/calculator
```

### Extras

**Install the Kubernetes Dashboard (UI)**

```powershell
PS> kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v1.10.1/src/deploy/alternative/kubernetes-dashboard.yaml
...
deployment.apps "kubernetes-dashboard" created
service "kubernetes-dashboard" created
```

View the Kubernetes Dashboard

```powershell
PS> kubectl proxy
Starting to serve on 127.0.0.1:8001
```

Visit http://localhost:8001/api/v1/namespaces/kube-system/services/kubernetes-dashboard/proxy in the browser.

### Try some things

Scale out the service to increase parallel processing of messages

```
kubectl scale --replicas=3 deployment/calculator
```

Roll out an upgrade and note that there is 0 downtime

```
kubectl apply -f deploy/calculator.yaml
```

Kill the service (simulating a crash) and watch it recover

```
kubectl delete po --selector=app=webui
```

View application logs

```
kubectl logs --selector=app=webui
# stream the logs:
kubectl logs -f $(kubectl get po --selector=app=webui--output=jsonpath={.items..metadata.name})
```

