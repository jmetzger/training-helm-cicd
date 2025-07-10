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
# Recherchiere wie die Werte gesetzt werden (artifacthub.io) oder verwende die folgenden Befehle:
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
helm upgrade --install my-nginx bitnami/nginx --create-namespace --namespace app-<nameskuerzel> -f prod/values.yaml  
```

### Umschauen 

```
# Wo speichert er Information, die er später mit helm get abruft
kubecl -n app-<namenskuerzel> get secrets
```

```
kubectl -n app-<namenskuerzel> get pods
helm -n app-<namenskuerzel> status my-nginx 
helm -n app-<namenskuerzel> list
# alle helm charts anzeigen, die im gesamten Cluster installierst wurden 
helm -n app-<namenskuerzel> list -A
helm -n app-<namenskuerzel> history my-nginx 
```

### Umschauen get 

```
helm -n app-tln1 get values my-nginx
helm -n app-tln1 get manifest my-nginx
helm -n app-tln1 get manifest my-nginx | grep "150Mi" -A4 -B4 
# Can I see all values use -> YES
# Look for COMPUTED VALUES in get all ->
helm -n app-tln1 get all my-nginx 
```

### Uninstall 

```
helm -n app-<namenskuerzel> uninstall my-nginx 
# namespace wird nicht gelöscht
# händisch löschen
kubectl delete ns app-<namenskuerzel>
# crd's werden auch nicht gelöscht 
```

## Problem: OutOfMemory (OOM-Killer) if container passes limit in memory 

  * if memory of container is bigger than limit an OOM-Killer will be triggered
  * How to fix. Use memory limit in the application too !
    * https://techcommunity.microsoft.com/blog/appsonazureblog/unleashing-javascript-applications-a-guide-to-boosting-memory-limits-in-node-js/4080857
