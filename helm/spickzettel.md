# Spickzettel helm (Wichtigste Befehle) 

## Alle helm-releases anzeigen 

```
# im eigenen Namespace 
helm list
# in allen Namespaces
helm list -A
# für einen speziellen
helm -n kube-system list 
```

## Helm - Chart installieren 

```
# Empfehlung mit namespace
# Repo hinzufügen für Client 
helm repo add bitnami https://charts.bitnami.com/bitnami
helm install my-nginx bitnami/nginx --version 19.0.1 --create-namespace --namespace=app-<namenskuerzel>
```

## Helm - Suche  

```
# welche Repos sind konfiguriert
helm repo list
helm search repo bitnami
helm search hub
```

## Helm - template 

```
# Rendern des Templates
helm repo add bitnami https://charts.bitnami.com/bitnami
helm template my-nginx bitnami/nginx
helm template bitnami/nginx
```

## Helm - values (von installierter Release aus secrets) 

```
helm -n app-jm get values my-nginx 
helm -n app-jm get values my-nginx --revision 1
```

## Helm - Hilfe 

```
# Hilfe ist auf jeder Ebene möglich 
helm --help
helm get --help
helm get values --help
```
