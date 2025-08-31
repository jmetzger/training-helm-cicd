# scc in OpenShift (ocp) 

## Whoami (which user) 

```
oc whoami
```

<img width="422" height="57" alt="image" src="https://github.com/user-attachments/assets/0ee087b7-0621-4bab-8427-a5b1ad3192fd" />
```

```
system:admin
```



## Show available scc's 

```
oc get scc
# nur die namen 
oc get scc --no-headers -o custom-columns=NAME:.metadata.name
oc get scc --no-headers -o custom-columns=NAME:.metadata.name,PRIORITY:.priority
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

## Who can use privileged 

```
oc policy who-can use scc privileged
# Whats within kube-system 
oc -n kube-system policy who-can use scc privileged
```

## Which scc is used by a pod ?

```
oc get pods -n default -o custom-columns=NAME:.metadata.name,SCC:.metadata.annotations.openshift\.io/scc
```



