# template und template --debug  

## template 

### Warum ?

  * Ich will vorher sehen, wie mein Manifest ausschaut, bevor ich es zum Api-Server schicke.

### Was macht das ? 

  * Rendered das Template.

### Was macht es nicht ? 

  * Überprüft NICHT anhand der API-Definition
    * z.B. sind die Felder richtig gesetzt bei Deployment
   
### Beispiel: 

```
helm repo add bitnami https://charts.bitnami.com/bitnami
# Kann sehr lang sein 
helm -n app-<namenskuerzel> template my-nginx bitnami/nginx --version 19.0.4 | less

helm -n app-jm template my-nginx bitnami/nginx --version 19.0.4 | grep -A 4 -i ^Kind

```

## template --debug 

### Warum ? 

  * Zeigt mein template auch an, wenn ein yaml-Einrückungsfehler oder Syntax - fehler da ist. 

### Beispiel 

```
helm -n app-jm template my-nginx bitnami/nginx --version 19.0.4 --debug
```
    
