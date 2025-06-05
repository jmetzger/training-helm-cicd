# Helm Template (Rendering) - Ablauf und Kontext

## Ablauf 

![Helm Template - Rendering Ablauf](/images/helm-template-ablauf.svg)

## Aufbau des Kontext:

  * Helm baut einen Kontext auf:
  * bedeutet bei Helm, dass für jedes Chart (und Subchart) eine Datenstruktur erstellt wird, die die Templates beim Rendern als Eingabe verwenden.

### Struktur des Kontext

```
├── Chart:
│   ├── Name: mychart
│   └── Version: 0.1.0
├── Release:
│   ├── Name: myrelease
│   └── Namespace: default
├── Values:
│   ├── image:
│   │   └── tag: 1.2.3
│   └── global:
│       └── commonLabels: {...}
├── Capabilities:
│   └── APIVersions: [...]
├── Files:
│   └── get: function to read files/
└── Template:
    └── Name: templates/deployment.yaml
```
