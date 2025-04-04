# Whitespace-Kontrolle

## Grundlagen 

  * In Helm (bzw. in Go-Templates) hast du verschiedene Möglichkeiten, den Umgang mit Whitespace (z. B. Leerzeichen, Zeilenumbrüche) zu steuern:

- `{{ ... }}`:  
  Standardvariante. Lässt den Whitespace außerhalb der geschweiften Klammern unverändert.

- `{{- ... }}`:  
  Entfernt den Whitespace links (vor) dem Ausdruck.  

- `{{ ... -}}`:  
  Entfernt den Whitespace rechts (nach) dem Ausdruck.  

- `{{- ... -}}`:  
  Entfernt Whitespace sowohl links als auch rechts des Ausdrucks.

## Übung

```
cd
mkdir -p helm-exercises
cd helm-exercises
mkdir 01-whitespaces 
cd 01-whitespaces
```

```
nano Chart.yaml
```


```
apiVersion: v2
name: whitespaces-test 
version: 0.1.0
appVersion: 1.0.0
```


```
nano whitespace-test.tpl
```


   ```gotemplate
   apiVersion: v1
   kind: ConfigMap
   metadata:
     name: whitespace-test
   data:
     # Beispiel ohne Whitespace-Kontrolle
     key1: "value1" {{ .Chart.Name }}
     # Beispiel mit Whitespace-Kontrolle (links entfernt)
     key2: "value2" {{- .Chart.Name }}
     # Beispiel mit Whitespace-Kontrolle (rechts entfernt)
     key3: "value3" {{ .Chart.Name -}}
     # Beispiel mit Whitespace-Kontrolle (links und rechts entfernt)
     key4: "value4" {{- .Chart.Name -}}
   ```

```
helm template .
```

