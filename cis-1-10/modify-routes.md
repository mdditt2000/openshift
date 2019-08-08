#!/bin/bash

# Container Ingress Services 1.10 

CIS build: amit49g/k8s-bigip-ctlr:latest
AS3: 3.11.0-3
BIG-IP 14.1
 
* Install AS3 on BIGIP
https://clouddocs.f5.com/products/extensions/f5-appsvcs-extension/latest/userguide/installation.html

* AS3/CCCL arguments:
Add extra argument in the CIS deployment if desired to use “as3” path  
 --agent=as3
To use “cccl” path then use the below argument
 --agent=cccl

* If you using BIGIP self-signed certs please
 --insecure=true

Example f5-k8s-bigip-ctlr-openshift.yaml

#Note: CCCL will be removed in the upcoming months. CCCL doesnt support BIG-IP v14.1

## Create openshift BIGIP controller authentication and RBAC
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
oc create -f f5-demo-app-route-health.yaml -n f5demo
oc create -f f5-demo-app-route-balance.yaml -n f5demo
oc create -f f5-demo-app-route-edge-ssl.yaml -n f5demo
```
## Delete container f5-demo-app-route
```
oc delete -f f5-demo-app-route-deployment.yaml -n f5demo
oc delete -f f5-demo-app-route-service.yaml -n f5demo
oc delete -f f5-demo-app-route-basic.yaml -n f5demo
oc delete -f f5-demo-app-route-balance.yaml -n f5demo
oc delete -f f5-demo-app-route-edge-ssl.yaml -n f5demo
```
Coming next
* re-encrypt
* passthrough