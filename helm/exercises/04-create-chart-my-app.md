# Chart my-app erstellen 

## Chart erstellen 

```
cd 
mkdir my-charts
cd my-charts
```

```
helm create my-app
``` 

## Install helm - chart 

```
# Variante 1:
helm install my-app-release my-app
```

```
# Variante 2:
cd my-app
helm install my-app-release .
```

```
kubectl get all
kubectl get pods 
```
