# scc in OpenShift (ocp) 

## Show available scc's 

```
oc get scc
# nur die namen 
oc get scc --no-headers -o custom-columns=NAME:.metadata.name
```

```
restricted - Sehr restriktiv (Standard)
restricted-v2 - Neue Version von restricted
anyuid - Erlaubt beliebige User IDs
nonroot - Muss als non-root laufen
nonroot-v2 - Neue Version von nonroot
hostaccess - Host-Zugriff erlaubt
hostmount-anyuid - Host-Mounts + beliebige UIDs
hostnetwork - Host-Netzwerk-Zugriff
hostnetwork-v2 - Neue Version
node-exporter - Für Node Exporter
privileged - Vollzugriff
```

```
# Who can use privileged
oc policy who-can use scc privileged
```

