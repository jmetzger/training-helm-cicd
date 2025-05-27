# Exercise cert-manager - CRD's 

## Installieren 

```
mkdir cm-values
cd cm-values
nano values.yaml
```

```
crds:
  enabled: true
```

```
helm repo add jetstack https://charts.jetstack.io
helm repo update
helm install cert-manager jetstack/cert-manager --namespace cert-manager-<namenskuerzel> --create-namespace -f values.yaml
```

## CRD's da ? 

```
kubectl get crds
```


## Deinstallieren 

```
helm -n cert-manager-<namenskuerzel> uninstall cert-manager
```

## CRD's noch da ? 

```
kubectl get crds
```


## CRD's händisch löschen 

```
kubectl delete crd ...
```
