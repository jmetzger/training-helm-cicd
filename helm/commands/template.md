# template und template --debug  

## template 

### Warum ?

  * Ich will vorher sehen, wie mein Manifest ausschaut, bevor ich es zum Kube-API-Server schicke.

### Was macht das ? 

  * Rendered das Template.

### Was macht es nicht ? 


  * Da er erst nicht an den schickt,
  * Überpüft er nicht, ob der Syntax korrekt ist, nur ob das yaml-format eingehalten wird  
   
### Beispiel: 

```
helm repo add bitnami https://charts.bitnami.com/bitnami
# Kann sehr lang sein 
helm -n app-<namenskuerzel> template my-nginx bitnami/nginx --version 19.0.4 | less
helm -n app-<namenskuerzel> template my-nginx bitnami/nginx --version 19.0.4 | grep -A 4 -i ^Kind

```

## template --debug 

### Warum ? 

  * Zeigt mein template auch an, wenn ein yaml-Einrückungsfehler oder Syntax - fehler da ist. 

### Beispiel 

```
helm -n app-jm template my-nginx bitnami/nginx --version 19.0.4 --debug
```
    
