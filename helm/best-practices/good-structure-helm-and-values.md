# Helm - good structure for helm-charts and helm-values 

  * e.g. in git repo

```
helm-exercises/
├── helm-values
│ ├── iftest
│ │ └── values.yaml
│ └── my-dep
│ └── values.yaml
└── iftest
├── Chart.yaml
├── charts
├── templates
│ ├── _helpers.tpl
│ └── cm.yaml
└── values.yaml
```
