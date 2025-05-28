# Name Template Exercise 

## Prerequisites range-example 

  * helm-exercises/range chart exists 


## Step 1: Put in file starting _helpers.tpl 

```
{{/* Definiere ein named template namens "mychart.containerPort" */}}
{{- define "range.containerPort" -}}
- name: http
  containerPort: 80
  protocol: TCP
{{- end }}
```

## Step 2: templates/deployment.yaml 

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: test-deploy
spec:
  replicas: 1
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx 
    spec:
      containers:
        - name: web
          image: "nginx:latest"
          ports:
            {{ include "range.containerPort" . }}
```

```
helm template ..
```
