# Type Conversions: toYaml 

## Exercise 

```
cd
mkdir -p helm-exercises
cd helm-exercises
helm create example-toyaml 
cd example-toyaml
rm -fR values.yaml
rm -fR templates/*
```

```
nano templates/configmap.yaml  
```

```
apiVersion: v1
kind: ConfigMap
metadata:
  name: {{ .Release.Name }}-config
data:
  app-config.yaml: |
    {{- toYaml .Values.appConfig | nindent 4 }}
```

```
nano values.yaml  
```

```
appConfig:
  server:
    port: 8080
    host: "0.0.0.0"
  features:
    auth: true
    metrics: true
  database:
    user: "admin"
    password: "secret"
    hosts:
      - db1.example.com
      - db2.example.com
```

```
helm template .
```


## Ref: 
 
  * https://helm.sh/docs/chart_template_guide/function_list/#type-conversion-functions
