#!/bin/bash

CIS build: subbuv26/k8s-bigip-ctlr:as3routes_path
AS3: 3.11.0-3
 
To use the above build:
Add extra argument in the CIS deployment if desired to use “as3” path  
 --agent=as3
To use “cccl” path then use the below argument
--agent=cccl