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
  - name: redis
    version: "18.0.0"
    repository: "https://charts.bitnami.com/bitnami"
  - name: nginx
    version: "21.x.x"
    repository: "https://charts.bitnami.com/bitnami"
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

## Exercise 2: Werte in den Untercharts setzen (nginx und redis) 

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
helm template . | grep -i '^Kind' -A 4
```

## Exercise 3: Create chart with condition 

```
nano Chart.yaml
```

```
# change dependency block
# adding condition 
dependencies:
  - name: redis
    version: "18.3.2"
    repository: "https://charts.bitnami.com/bitnami"
    condition: redis.enabled
```

```
nano values.yaml
```

```
# unten anfügen 
redis:
  enabled:
    false
```

```
helm template .
```

```
# values-file anlegen
cd
cd helm-exercises
mkdir -p helm-values
cd helm-values
mkdir my-dep
cd my-dep
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
helm template my-dep -f helm-values/my-dep/values.yaml
helm template my-dep -f helm-values/my-dep/values.yaml | grep kind -A 2
```
