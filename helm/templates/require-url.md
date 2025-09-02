# Require url 

## Walkthrough 

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

```
# labels ergänzen (4 spaces eingerueckt)
   url: {{ required "Please set url" .Values.deployment.url | quote }}
```

```
cd
mkdir -p helm-values
cd helm-values
mkdir -p prod
mkdir -p dev
nano prod/values.yaml
```

```
deployment:
  url: 'https://someprod.com"
```

```
nano dev/values.yaml
```

```
deployment:
  url: 'https://somedev.com'
```

## Testing 

```
cd
cd helm-exercises
```

```
# ohne values 
helm template urlexample
```

```
# mit prod  values
helm template urlexample -f ../helm-values/prod/values.yaml
```

```
helm template urlexample -f ../helm-values/dev/values.yaml
```


