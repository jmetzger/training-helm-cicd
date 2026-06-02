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
helm template .
```

## Exercise 2: Werte in den Subcharts setzen

Werte für Subcharts werden im Parent `values.yaml` unter dem jeweiligen Chart-Namen gesetzt.

```
nano values.yaml
```

```
nginx:
  fullnameOverride: nginx
  replicaCount: 2
redis:
  fullnameOverride: redis
  replicaCount: 1
```

```
helm template .
helm template . | grep "replicas:"
```

## Exercise 3: Globale Werte (globals) verwenden

Globals werden einmal im Parent gesetzt und fließen **automatisch in alle Subcharts** — ohne den Subchart-Namen als Prefix.

```
nano values.yaml
```

```
global:
  imageRegistry: "my-private-registry.example.com"

nginx:
  fullnameOverride: nginx
  replicaCount: 2
redis:
  enabled: false
  fullnameOverride: redis
  replicaCount: 1
```

```
helm template . | grep "image:"
```

Beide Subcharts (nginx und redis) verwenden jetzt `my-private-registry.example.com` als Registry — obwohl der Wert nur einmal gesetzt wurde. Das ist der Unterschied zu normalen Subchart-Values:

| | Subchart-Value | Global |
|---|---|---|
| Syntax | `redis.someValue` | `global.someValue` |
| Geltungsbereich | nur dieser Subchart | alle Subcharts |
