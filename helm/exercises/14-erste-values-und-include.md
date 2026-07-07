# Erste Values und include (von statisch zu dynamisch) 

## Prerequisites 

[Chart anlegt](/helm/exercises/09-create-helm-chart-from-scratch-deployment.md)

## Übung (Teil 1) 

```
# Die deployment.yaml
nano deployment.yaml
```

```
# replicas: 8 -> ersetzen durch
replicas: {{ .Values.replicas }}
```

```
cd ..
nano values.yaml
```

```
# fügen ganz unten hinzu
replicas: 2
```

```
helm upgrade --install app .
kubectl get pods
```

## Übung (Teil 2) 

```
cd templates
# mich interessiert app.fullname 
head -n 20 _helpers.tpl
nano deployment.yaml
```

```
# ersetze name: deployment-nginx -> durch ->
name: {{ include "app.fullname" . }}

# unter metadata:
# name: -> genauso eingerückt
labels:
    {{- include "app.labels" . | nindent 4 }}
```

```
cd ..
helm upgrade --install app .
helm list
kubectl get deploy, pods
```









