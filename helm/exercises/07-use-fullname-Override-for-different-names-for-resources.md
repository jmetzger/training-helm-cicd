# Use fullnameOverride 

```
cd
mkdir my-values
cd my-values
mkdir my-new-app
cd my-new-app
```

```
nano values.yaml
```

```
fullnameOverride: my-new-app
```

```
helm upgrade --install my-new-app training/my-app -f values.yaml
```
