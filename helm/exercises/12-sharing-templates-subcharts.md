# Templates mit Subcharts teilen

## Hintergrund

In Helm sind alle `define`-Bloecke **global sichtbar** — egal ob sie im Parent-Chart oder in einem
Subchart stehen. Das erlaubt es, Templates einmal zu definieren und ueberall zu nutzen.

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

## Schritt 2: Shared Templates im Parent definieren

```
# vi templates/_helpers.tpl
```

Folgendes am Ende der Datei **ergaenzen** (bestehenden Inhalt behalten):

```
{{- define "sharing.labels.simple" -}}
app: myapp
{{- end }}

{{- define "sharing.labels.detailed" -}}
app: myapp
managed-by: helm
environment: {{ .Values.global.env | default "dev" }}
{{- end }}
```

## Schritt 3: Dynamische Template-Referenz mit include

```
# vi values.yaml
```

Folgendes ergaenzen:

```
global:
  env: production
  labelStyle: detailed
```

Parent-Template anlegen:

```
# vi templates/configmap.yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: parent-config
  labels:
    {{- $mytemplate := printf "sharing.labels.%s" .Values.global.labelStyle }}
    {{- include $mytemplate . | nindent 4 }}
data:
  style: {{ .Values.global.labelStyle }}
```

```
helm template .
```

**Warum `include` statt `template`?**

`template "sharing.labels.detailed" .` ist ein String-Literal — der Name ist fest im Template
verdrahtet. Mit `include $mytemplate .` kann der Template-Name zur Laufzeit berechnet werden,
z.B. per `printf`. Das ergibt einen auswaehlbaren Template-Namen ohne Code-Aenderung.

## Schritt 4: Template aus dem Subchart heraus nutzen (Cross-Chart)

Der Subchart kann die im Parent definierten Templates verwenden — ohne eigene Definition.

```
# vi charts/myapp/templates/configmap.yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: subchart-config
  labels:
    {{- $mytemplate := printf "sharing.labels.%s" (.Values.global.labelStyle | default "simple") }}
    {{- include $mytemplate . | nindent 4 }}
data:
  info: "dieses Label wurde im Parent-Chart definiert"
```

```
helm template .
```

Das Template `sharing.labels.detailed` ist im Subchart verfuegbar, obwohl es nur im Parent
`templates/_helpers.tpl` steht — **Templates sind global in Helm**.

## Schritt 5: labelStyle wechseln

```
helm template . --set global.labelStyle=simple
```

Jetzt wird automatisch `sharing.labels.simple` fuer beide Charts (Parent + Subchart) verwendet.
