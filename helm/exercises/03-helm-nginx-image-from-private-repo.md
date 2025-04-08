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
  security:
     allowInsecureImages: true


image:
  registry: "registry.do.t3isp.de"
  repository: nginx
  # tag: 1.27.4
  pullSecrets:
    - regcred-do

extraDeploy:
  - apiVersion: v1
    data:
      .dockerconfigjson: <gibts-from-trainer>
    kind: Secret
    metadata:
       name: regcred-do
    type: kubernetes.io/dockerconfigjson

```



```
cd
cd manifests/nginx-values
helm upgrade --install my-nginx bitnami/nginx -f prod/values.yaml
```
