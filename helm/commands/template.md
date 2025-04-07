# Template 

## Warum ?

  * Ich will vorher sehen, wie mein Manifest ausschaut, bevor ich es zum Api-Server schicke.

## Was macht das ? 

  * Rendered das Template.

## Was macht es nicht ? 

  * Überprüft NICHT anhand der API-Definition
    * z.B. sind die Felder richtig gesetzt bei Deployment
   
## Beispiel: 

```
helm repo add bitnami https://charts.bitnami.com/bitnami
# Kann sehr lang sein 
helm -n app-<namenskuerzel> template my-nginx bitnami/nginx --version 19.0.4 | less 
```

    
