# Create umbrella-chart 

## Exercise 1: Create chart & cleanup with Dependency 

```
cd 
mkdir -p helm-exercises 
cd helm-exercises 
helm create my-umbrella-chart
cd my-umbrella-chart 
rm -fR templates
rm values.yaml
touch values.yaml 
```

```
nano Chart.yaml
```

```
# Add dependencies 
dependencies:
  - name: nginx
    version: "0.12.x"
    repository: "oci://registry-1.docker.io/cloudpirates"
  - name: redis
    version: "0.29.x"
    repository: "oci://registry-1.docker.io/cloudpirates"
```

```
# Das 1. Mal - dann wird Chart.lock angelegt 
helm dependency update
ls -la Chart.lock 
```

```
rm -fR charts
helm dependency build
```

```
helm dependency --help 
## what is the difference 
```

```
helm template .
```

## Exercise 2: Werte in den Subcharts setzen (nginx und redis) 

```
nano values.yaml
```

```
nginx:
  fullnameOverride: nginx
redis:
  fullnameOverride: redis
```

```
helm template .
helm template . | grep -i '^kind' -A 4
```

## Exercise 3: Create chart with condition 

```
nano Chart.yaml
```

```
# change dependency block
# adding condition 
dependencies:
  - name: nginx
    version: "0.12.x"
    repository: "oci://registry-1.docker.io/cloudpirates"
  - name: redis
    version: "0.29.x"
    repository: "oci://registry-1.docker.io/cloudpirates"
    condition: redis.enabled
```

```
nano values.yaml
```

```
nginx:
  fullnameOverride: nginx
redis:
  enabled: false
  fullnameOverride: redis
```

```
helm template .
# redis-Ressourcen (StatefulSet, Secret) sollten nicht mehr erscheinen
helm template . | grep "^kind:" 
```

```
# values-file anlegen um redis wieder einzuschalten
cd
cd helm-exercises
mkdir -p helm-values/my-umbrella-chart
cd helm-values/my-umbrella-chart
```

```
nano values.yaml
```

```
redis:
  enabled: true
```

```
cd
cd helm-exercises
helm template my-umbrella-chart -f helm-values/my-umbrella-chart/values.yaml
helm template my-umbrella-chart -f helm-values/my-umbrella-chart/values.yaml | grep "^kind:"
```

## Exercise 4: Globale Werte (globals) verwenden

Globals sind Werte, die im Parent-Chart unter `global:` gesetzt werden und **automatisch in allen Subcharts** als `.Values.global.*` verfügbar sind – ohne explizite Weitergabe.

```
cd
cd helm-exercises/my-umbrella-chart
mkdir -p templates
```

```
nano templates/configmap-global.yaml
```

```
apiVersion: v1
kind: ConfigMap
metadata:
  name: global-config
data:
  environment: {{ .Values.global.environment | default "unknown" }}
```

```
nano values.yaml
```

```
global:
  environment: "training"

nginx:
  fullnameOverride: nginx
redis:
  enabled: false
  fullnameOverride: redis
```

```
helm template .
# Nur das ConfigMap anzeigen
helm template . | grep -A 5 "global-config"
```

Das `global.environment` ist in **jedem Subchart** als `.Values.global.environment` verfügbar – ohne dass der Wert explizit weitergegeben werden muss. Subcharts können eigene `global`-Defaults in ihrer `values.yaml` definieren, der Parent-Wert hat aber immer Vorrang.
