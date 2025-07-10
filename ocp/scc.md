# scc in OpenShift (ocp) 

## Show available scc's 

```
oc get scc
# nur die namen 
oc get scc --no-headers -o custom-columns=NAME:.metadata.name
```
