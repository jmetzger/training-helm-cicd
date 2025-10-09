#  Install and upgrade of release 

## Schritt 1: Install (wir rennen in Fehler wg. bitnami)

```
helm repo add bitnami https://charts.bitnami.com/bitnami
```

```
# Schritt 1: Testen  
helm upgrade --install my-nginx bitnami/nginx --version 19.0.4 --create-namespace --reset-values --namespace app-<namenskuerzel> --dry-run
```
```
# Schritt 2: Installieren 
helm upgrade --install my-nginx bitnami/nginx --version 19.0.4 --create-namespace --reset-values --namespace app-<namenskuerzel>
```

```
# überprüfen // laufen die pods 
kubectl -n app-<namenskuerzel> get pods 
```

```
# Bei fehler für pod
kubectl -n app-<namenskuerzel> describe pods <tab><tab>
```

## Schritt 2: Alternative CloudPirates / relativ frisch 

  * Dokumentation ist noch nicht perfekt in diesem Projekt

## Schrit 2.1: cloudPirates, der 1. Fehler 

```
# Mini-Step 1: Testen 
helm upgrade --install my-nginx oci://registry-1.docker.io/cloudpirates/nginx --reset-values --namespace app-<namenskuerzel> --create-namespace --version 0.1.14 --dry-run
```

```
# Mini-Step 2: Installieren 
helm upgrade --install my-nginx oci://registry-1.docker.io/cloudpirates/nginx --reset-values --namespace app-<namenskuerzel> --create-namespace --version 0.1.14 
```

```
# Geht das denn auch ?
kubectl -n app-<namenskuerzel> get pods
```

<img width="1051" height="78" alt="image" src="https://github.com/user-attachments/assets/7c9144d0-57e5-4380-8260-86df731b29c5" />

```
# Problem CrashLoopBackoff
# Step 1: Sehen wir was im Describe ? Nope -> nicht mehr als vorher 
kubectl -n app-<namesnkuerzel> describe pods
```

```
# Was sagen die Logs
kubectl -n app-<namesnkuerzel> logs my-<tab><tab>
```

<img width="1222" height="68" alt="image" src="https://github.com/user-attachments/assets/ab07dde1-f96b-4349-a9bd-a7e52a25cdbc" />

```
# Der port 80 kann nicht geöffnet werden, wenn man unprivileged läuft
```

## Schritt 2.2: CloudPirates ... Er läuft aber nicht ready 


   * Wir nehmen das Beispiel aus der Doku (Spoiler-Alert, leider nicht komplett richtig)
   * https://artifacthub.io/packages/helm/cloudpirates-nginx/nginx

```
cd
mkdir -p helm-values/nginx
cd helm-values/nginx
nano values.yaml
```

```
# my-values.yaml
serverConfig: |
  server {
    listen 0.0.0.0:8080;
    root /usr/share/nginx/html;
    index index.html index.htm;
    
    location / {
      try_files $uri $uri/ /index.html;
    }
  }
livenessProbe:
  type: httpGet
  path: /
readinessProbe:
  type: httpGet
  path: /
```

```
# Mini-Step 2: Installieren 
helm upgrade --install my-nginx oci://registry-1.docker.io/cloudpirates/nginx --reset-values --namespace app-<namenskuerzel> --create-namespace -f values.yaml --version 0.1.14 
```

## Schritt 2.2: CloudPirates ... Readiness und LivenessCheck 



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
helm upgrade --install my-nginx bitnami/nginx --create-namespace --namespace app-<nameskuerzel> --version 21.0.6 -f prod/values.yaml  
```

### Umschauen 

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
# Wo speichert er Information, die er später mit helm get abruft
kubectl -n app-<namenskuerzel> get secrets
```


```
helm -n app-tln1 get values my-nginx
helm -n app-tln1 get manifest my-nginx
helm -n app-tln1 get manifest my-nginx | grep "150Mi" -A4 -B4 
# Can I see all values use -> YES
# Look for COMPUTED VALUES in get all ->
helm -n app-tln1 get all my-nginx 
```

```
# Hack COMPUTED VALUES anzeigen lassen
# Welche Werte (values) hat er zur Installation verwendet
helm -n app-<namenskuerzel> get all my-nginx | grep -i computed -A 200

```

## Tipp: values aus alter revision anzeigen 

```
# Beispiel: 
helm -n app-jm get values  my-nginx --revision 1
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
