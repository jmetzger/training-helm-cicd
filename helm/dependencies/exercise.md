# Using Dependencies 

## Exercise 1: Create chart with Dependency 

```
cd 
mkdir -p helm-exercises 
cd helm-exercises 
helm create my-dep
cd my-dep
nano Chart.yaml 
```

```
# Add dependencies 
dependencies:
  - name: redis
    version: "0.9.x"
    repository: "oci://registry-1.docker.io/cloudpirates"
```

```
# Das 1. Mal - dann wird Chart.lock angelegt 
helm dependency update
ls -la Chart.lock
ls -la charts
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

## Exercise 2: Create chart with condition 

```
nano Chart.yaml
```

```
# change dependency block
# adding condition 
dependencies:
  - name: redis
    version: "0.9.x"
    repository: "oci://registry-1.docker.io/cloudpirates/"
    condition: redis.enabled
```

```
nano values.yaml
```

```
# unten anfügen 
redis:
  enabled: false
```

```
helm template .
```

```
# values-file anlegen
cd
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
helm template my-dep -f ../helm-values/my-dep/values.yaml
helm template my-dep -f ../helm-values/my-dep/values.yaml | grep kind -A 2
```

## Exercise 3: Redis nach draussen öffnen (Werte in subchart setzen) 

```
# Er soll immer nach draussen lauschen
cd
cd helm-exercises/my-dep
nano values.yaml
```

```
redis:
  enabled: false
  service:
    type: LoadBalancer 
  
```

```
cd ..
helm template my-dep -f ../helm-values/my-dep/values.yaml | grep kind -A 2
helm upgrade --install app-with-redis my-dep -f ../helm-values/my-dep/values.yaml
```

```
helm list
kubectl get svc
```

## Exercise 4 

```
cd my-dep
nano Chart.yaml
```

```
# Weiteres chart mit - ergänzen
```

```
dependencies:
  - name: redis
    version: "0.9.x"
    repository: "oci://registry-1.docker.io/cloudpirates/"
    condition: redis.enabled
  - name: redis
    version: "0.9.x"
    repository: "oci://registry-1.docker.io/cloudpirates/"
    alias: redis-intern
```

```
nano values.yaml
```

```
# unten ergänzen 
redis-intern:
  service:
    type: NodePort
```

```
cd ..
helm template my-dep -f ../helm-values/my-dep/values.yaml | grep type
helm upgrade --install app-with-redis my-dep -f ../helm-values/my-dep/values.yaml
```

