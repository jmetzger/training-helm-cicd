# Require url 

```
cd
mkdir -p helm-exercises
cd helm-exercises
helm create urlexample
cd urlexample
nano values.yaml
```

```
# ganz am Ende anhängen 
deployment:
  url: ''
```

```
cd templates
nano deployment.yaml
```


