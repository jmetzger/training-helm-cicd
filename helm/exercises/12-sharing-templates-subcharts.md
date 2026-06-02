# Templates mit Subcharts teilen

## Hintergrund

In Helm sind alle `define`-Bloecke **global sichtbar** — egal ob sie im Parent-Chart oder in einem
Subchart stehen. Parent und Subcharts koennen dieselben Templates nutzen.

Wichtiger Unterschied zwischen `template` und `include`:

| | `template "name" .` | `include $var .` |
|---|---|---|
| Dynamische Referenz | Nein — nur String-Literal | Ja — Variable moeglich |
| Ergebnis pipebar (nindent etc.) | Nein | Ja |

## Schritt 1: Chart mit lokalem Subchart anlegen

```
cd
mkdir -p helm-exercises
cd helm-exercises
helm create sharing-templates
cd sharing-templates
```

Subchart-Verzeichnis anlegen:

```
mkdir -p charts/myapp/templates
```

```
# vi charts/myapp/Chart.yaml
apiVersion: v2
name: myapp
version: 0.1.0
```

## Schritt 2: Shared Template im Parent definieren

```
# vi templates/_helpers.tpl
```

Folgendes am Ende der Datei **ergaenzen** (bestehenden Inhalt behalten):

```
{{- define "mychart.labels" -}}
app: myapp
managed-by: helm
{{- end }}
```

## Schritt 3: Template dynamisch per Variable einbinden

```
# vi templates/configmap.yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: parent-config
  labels:
    {{- $mytemplate := "mychart.labels" }}
    {{- include $mytemplate . | nindent 4 }}
```

```
helm template .
```

Die Variable `$mytemplate` haelt den Template-Namen als String. `include` loest die Variable auf —
`template` koennte das nicht, es erwartet einen String-Literal direkt.

## Schritt 4: Template aus dem Subchart heraus nutzen (Cross-Chart)

Der Subchart kann das im Parent definierte Template verwenden — ohne eigene Definition.

```
# vi charts/myapp/templates/configmap.yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: subchart-config
  labels:
    {{- $mytemplate := "mychart.labels" }}
    {{- include $mytemplate . | nindent 4 }}
data:
  info: "dieses Label wurde im Parent-Chart definiert"
```

```
helm template .
```

Das Template `mychart.labels` ist im Subchart verfuegbar, obwohl es nur im Parent
`templates/_helpers.tpl` steht — **Templates sind global in Helm**.
