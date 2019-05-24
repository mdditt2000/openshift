#!/bin/bash

#create CC route
oc create -f f5-demo-app-route-basic.yaml -n f5demo
oc create -f f5-demo-app-route-balance.yaml -n f5demo
oc create -f f5-demo-app-route-health.yaml -n f5demo
oc create -f f5-demo-app-route-clientssl.yaml -n f5demo

#delete CC route
oc delete -f f5-demo-app-route-basic.yaml -n f5demo
oc delete -f f5-demo-app-route-balance.yaml -n f5demo
oc delete -f f5-demo-app-route-health.yaml -n f5demo
oc delete -f f5-demo-app-route-clientssl.yaml -n f5demo