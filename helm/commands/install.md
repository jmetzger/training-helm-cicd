#  Install and upgrade of release 

## Install 

```
helm repo add bitnami https://charts.bitnami.com/bitnami
```

```
# Installiert 
helm install my-nginx bitnami/nginx --version 19.0.4 --create-namespace --namespace app-<namenskuerzel>
# Zeigt an, was er ausrollen würde 
helm install my-nginx bitnami/nginx --version 19.0.4 --dry-run # auch für uninstall, upgrade 
```

```
# noch besser
# Installiert 
helm upgrade --install my-nginx bitnami/nginx --version 19.0.4 --create-namespace --namespace app-<namenskuerzel>
```

```
# überprüfen // laufen die pods 
kubectl -n app-<namenskuerzel> get all 
```

## Exercise: Upgrade to new version 

```
# Recherche wie die Werte gesetzt werden (artifacthub.io) oder
helm show values bitnami/nginx
helm show values bitnami/nginx | less
```

```
cd 
mkdir -p nginx-values 
cd nginx-values
mkdir prod
cd prod
```

```
nano values.yaml
```

```
resources:
   requests:
     cpu: 0.1
     memory: 150Mi
   limits:
     cpu: 0.1
     memory: 150Mi
```

```
cd ..
helm upgrade --install my-nginx bitnami/nginx --namespace app-<nameskuerzel> -f prod/values.yaml  
```

### Umschauen 

```
kubectl -n app-<namenskuerzel> get pods
helm -n app-<namenskuerzel> status my-nginx 
helm -n app-<namenskuerzel> list
helm -n app-<namenskuerzel> history 
```

### Uninstall 

```
helm -n app-<namenskuerzel> uninstall my-nginx 
# namespace wird nicht gelöscht 
# crd's werden auch nicht gelöscht 
```

## Problem: OutOfMemory (OOM-Killer) if container passes limit in memory 

  * if memory of container is bigger than limit an OOM-Killer will be triggered
  * How to fix. Use memory limit in the application too !
    * https://techcommunity.microsoft.com/blog/appsonazureblog/unleashing-javascript-applications-a-guide-to-boosting-memory-limits-in-node-js/4080857
