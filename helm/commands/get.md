# Get information from release 

```
# evtl namespace verwenden, wenn im Namespace installiert
helm get values my-nginx -n app-ms
```


```
# Zeige alle user geänderten values an 
helm get values my-nginx
# Zeige alle values, auch defaults an 
helm get values my-nginx --all
# Zeige angewendete manifests an 
helm get manifest my-nginx
# Zeige die hooks an, wenn vorhanden
helm get hooks my-nginx
# Zeige nochmals die Notes an
helm  get notes my-nginx


```
