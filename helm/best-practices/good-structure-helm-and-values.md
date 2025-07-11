# Helm - good structure for helm-charts and helm-values 

  * e.g. in git repo

##  Beispiel 1 

```
helm-exercises/
├── helm-values
│   ├── iftest
│   │   └── values.yaml
│   └── my-dep
│       └── values.yaml
└── iftest
    ├── Chart.yaml
    ├── charts
    ├── templates
    │   ├── _helpers.tpl
    │   └── cm.yaml
    └── values.yaml

```
## Beispiel 2: Struktur für gitlab ci/cd 

```
helm-repo-app1/
├── charts
│   └── app
│       ├── Chart.yaml
│       ├── templates
│       └── values.yaml
└── helm-values
    ├── prod
    │   └── values.yaml
    ├── staging
    │   └── values.yaml
    └── testing
        └── values.yaml
```


