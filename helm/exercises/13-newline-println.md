# Zeilenumbrueche in Go-Templates: println

## Hintergrund

In Helm Go-Templates gibt es mit `println` eine Funktion, die einen Zeilenumbruch (`\n`)
an einen String anhaengt. Das ist nuetzlich um z.B. mehrzeilige ConfigMap-Werte zu erzeugen.

## Schritt 1: Chart anlegen

```
cd
mkdir -p helm-exercises
cd helm-exercises
helm create newlines
cd newlines
rm -fR templates/*
```

## Schritt 2: Beispiel ohne Zeilenumbruch (Problem)

```
# vi templates/configmap.yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: {{ .Release.Name }}-env
data:
  app.env: |
    {{ "APP_ENV=production" }}{{ "LOG_LEVEL=info" }}
```

```
helm template .
```

**Was passiert:** Beide Werte kleben zusammen auf einer Zeile:

```
  app.env: |
    APP_ENV=productionLOG_LEVEL=info
```

## Schritt 3: Loesung mit println

`println` haengt ein `\n` an den String an:

```
# vi templates/configmap.yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: {{ .Release.Name }}-env
data:
  app.env: |
    {{ "APP_ENV=production" | println }}{{ "LOG_LEVEL=info" }}
```

```
helm template .
```

**Ergebnis:**

```
  app.env: |
    APP_ENV=production
    LOG_LEVEL=info
```

## Schritt 4: Achtung — Whitespace-Stripping entfernt den Zeilenumbruch

Das `-` in `{{-` oder `-}}` entfernt **allen** Whitespace inkl. Newlines.
Kombiniert man `println` mit `-}}`, wird der gerade eingefuegte `\n` sofort wieder gestrichen:

```
# vi templates/configmap.yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: {{ .Release.Name }}-env
data:
  app.env: |
    {{ "APP_ENV=production" | println -}}{{ "LOG_LEVEL=info" }}
```

```
helm template .
```

**Was passiert:** Das `-}}` strippt das `\n` von `println` — beide Zeilen kleben wieder:

```
  app.env: |
    APP_ENV=productionLOG_LEVEL=info
```

**Merksatz:** `println` und `-}}` in einem Block ausschliessen — das Minus macht den Newline rueckgaengig.

## Aufraeumen

```
cd
rm -fR helm-exercises/newlines
```

## Referenz

  * https://pkg.go.dev/text/template (println, print, printf)
  * https://helm.sh/docs/chart_template_guide/control_structures/#controlling-whitespace
