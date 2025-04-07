#  Install and upgrade of release 

## Install 

```
# Installiert 
helm install my-nginx bitnami/nginx --version 19.0.4 --create-namespace --namespace app-<namenskuerzel>
# Zeigt an, was er ausrollen würde 
helm install my-nginx bitnami/nginx --version 19.0.4 --dry-run # auch für uninstall, upgrade 
```

```
# noch besser
# Installiert 
helm upgrade --install my-nginx bitnami/nginx --version 19.0.4 --create-namespace --namespace app-<namenskuerzel>
```


## Exercise: Upgrade to new version 

```
cd 
mkdir -p nginx-values 
cd nginx-values
mkdir prod
cd prod
```

```
nano values.yaml
```

```
resources:
   requests:
     cpu: 0.1
       memory: 150Mi
     limits:
       cpu: 0.1
       memory: 150Mi
```

```
cd ..
helm upgrade --install my-nginx bitnami/nginx --namespace app-<nameskuerzel> -f prod/values.yaml  
```
