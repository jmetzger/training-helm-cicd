# Helm (bitnami/nginx) with image from private repo 

## Walkthrough 

```
cd
mkdir -p manifests
cd manifests
mkdir nginx-values
cd nginx-values
mkdir prod
cd prod 
nano values.yaml
```

```
global:
  imageRegistry: "registry.do.t3isp.de"
  imagePullSecrets:
    - regcred

extraDeploy:
  - apiVersion: v1
    data:
      .dockerconfigjson: <get-from-trainer>
    metadata:
       name: regcred
    type: kubernetes.io/dockerconfigjson


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
helm upgrade --install my-nginx bitnami/nginx -f prod/values
```
