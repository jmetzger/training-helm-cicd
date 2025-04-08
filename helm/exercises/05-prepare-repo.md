# Prepare a repo to be used for repo - server (standalone) web-server 

## Step 1 (all trainees and trainer) 

```
cd
cd my-charts/my-app
helm package . 
```

```
cd
mkdir my-helm-repo
mv my-charts/my-app/my-app-0.1.0.tgz my-helm-repo
helm repo index my-helm-repo --url https://registry.do.t3isp.de
```

## Only for trainer: Step 2 (copy these file to webserver registry.do.t3isp.de) 

  * e.g. nginx / /var/www/html
