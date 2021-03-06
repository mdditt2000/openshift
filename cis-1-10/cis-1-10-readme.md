# Container Ingress Services 1.10

CIS 1.10 has officailly posted. However here is some demo examples to provide PM with feedback. Please open issues on my github page on contact me at m.dittmer@f5.com

* CIS build: f5networks/k8s-bigip-ctlr:1.10.0
* AS3: 3.11.0-3
* BIG-IP 14.1

## Prerequisite 
 
* Install AS3 on BIGIP
https://clouddocs.f5.com/products/extensions/f5-appsvcs-extension/latest/userguide/installation.html

### AS3/CCCL arguments:

Add argument in the CIS controller deployment to use “as3” path  
 ```
 --agent=as3
 ```
To use “cccl” path then use the below argument
 ```
 --agent=cccl
```
If you using BIGIP self-signed certs please add or schema validation will fail
 ```
 --insecure=true
```
Example folder https://github.com/mdditt2000/openshift/tree/dev/cis-1-10

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
oc create -f f5-demo-app-route-basic.yaml -n f5demo
oc create -f f5-demo-app-route-balance.yaml -n f5demo
oc create -f f5-demo-app-route-edge-ssl.yaml -n f5demo
oc create -f f5-demo-app-route-reencrypt-ssl.yaml -n f5demo
oc create -f f5-demo-app-route-passthrough-ssl.yaml -n f5demo
```
Please look for example files in my repo

## Delete container f5-demo-app-route
```
oc delete -f f5-demo-app-route-deployment.yaml -n f5demo
oc delete -f f5-demo-app-route-service.yaml -n f5demo
oc delete -f f5-demo-app-route-basic.yaml -n f5demo
oc delete -f f5-demo-app-route-balance.yaml -n f5demo
oc delete -f f5-demo-app-route-edge-ssl.yaml -n f5demo
oc delete -f f5-demo-app-route-reencrypt-ssl.yaml -n f5demo
oc delete -f f5-demo-app-route-passthrough-ssl.yaml -n f5demo
```
## Notes

* insecureEdgeTerminationPolicy field spec is not working with reencrypt/passthrough route. This feature is not implemented in CC
* CIS not pushing configuration in BIGIP when there is a deletion of resource from BIGIP. CIS is source-of-truth

## Coming in CIS 1.11 scheduled for late September

* WAF policy support
* wildcards
* OpenShift 4.1