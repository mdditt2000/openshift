#!/bin/bash

CIS build: subbuv26/k8s-bigip-ctlr:as3routes_path
AS3: 3.11.0-3
 
To use the above build:
Add extra argument in the CIS deployment if desired to use “as3” path  
 --agent=as3
To use “cccl” path then use the below argument
--agent=cccl

## Validate the deployment
```
oc get deployments --namespace=kube-system
oc get replicasets --namespace=kube-system
oc get pods --namespace=kube-system
```
## Create kubernetes bigip container connecter, authentication and RBAC
```
oc create secret generic bigip-login --namespace kube-system --from-literal=username=admin --from-literal=password=admin
oc create serviceaccount bigip-ctlr -n kube-system
oc create -f f5-kctlr-openshift-clusterrole.yaml
oc create -f f5-k8s-bigip-ctlr-openshift.yaml
```
## Delete kubernetes bigip container connecter, authentication and RBAC
```
oc delete serviceaccount bigip-ctlr -n kube-system
oc delete -f f5-kctlr-openshift-clusterrole.yaml
oc delete -f f5-k8s-bigip-ctlr-openshift.yaml
oc delete secret bigip-login -n kube-system
```
## Create container f5-demo-app-route
```
oc create -f f5-demo-app-route-deployment.yaml -n f5demo
oc create -f f5-demo-app-route-service.yaml -n f5demo
oc create -f f5-demo-app-route-basic.yaml -n f5demo
oc create -f f5-demo-app-route-health.yaml -n f5demo
```