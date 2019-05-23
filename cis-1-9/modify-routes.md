#!/bin/bash

#create CIS route
oc create -f f5-demo-app-route-basic-configmap.yaml -n f5demo
oc create -f f5-demo-app-route-balance-configmap.yaml -n f5demo
oc create -f f5-demo-app-route-health-configmap.yaml -n f5demo
oc create -f f5-demo-app-route-nocookie-configmap.yaml -n f5demo

#delete CIS route
oc delete -f f5-demo-app-route-basic-configmap.yaml -n f5demo
oc delete -f f5-demo-app-route-balance-configmap.yaml -n f5demo
oc delete -f f5-demo-app-route-health-configmap.yaml -n f5demo
oc delete -f f5-demo-app-route-nocookie-configmap.yaml -n f5demo