# Using Dependencies 

## Step 1: Create chart 

```
cd 
cd -p helm-exercises 
helm create my-dep
cd my-dep
nano Chart.yaml 
```

```
# Add dependencies 
dependencies:
  - name: redis
    version: "18.0.0"
    repository: "https://charts.bitnami.com/bitnami"
```

```
# Das 1. Mal - dann wird Chart.lock angelegt 
helm dependency update
ls -la Chart.lock 
```

```
rm -fR charts
helm build update
```

