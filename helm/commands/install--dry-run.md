# Example dry-run 

```
helm repo add bitnami https://charts.bitnami.com/bitnami
# auch die gerenderten Manifeste werden angezeit auch ohne "--debug" 
helm -n app-jm install my-nginx bitnami/nginx --version 19.0.4 --dry-run
```
