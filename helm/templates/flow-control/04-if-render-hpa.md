# Render hpa only when enabled 

```
cd
mkdir -p helm-exercises

cd helm-exercises
helm create app2 
cd app2/templates 
less hpa.yaml
```

```
# find out the correct value to enable hpa 
cd ..
less values.yaml
```

```
# Seperate values from from helm-charts 
cd 
mkdir -p helm-values 
cd helm-values
mkdir prod
cd prod 
nano values.yaml
```

```
autoscaling:
    enabled: true
```
    
```
cd
cd helm-exercises 
helm template app2 -f ../helm-values/prod/values.yaml
```

