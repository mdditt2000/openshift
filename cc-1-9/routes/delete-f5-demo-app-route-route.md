#!/bin/bash

#delete container f5-demo-app-route
oc delete -f f5-demo-app-route-deployment.yaml -n f5demo
oc delete -f f5-demo-app-route-route.yaml -n f5demo