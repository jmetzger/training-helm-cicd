# Templating - spaces

## Explanation 

  * {{- -> trim on left side / INCLUDING new lines 
  * -}} -> trim on right side / ALSO: new lines 
  * trim tabs, whitespaces a.s.o. (see ref)

## Walkthrough 

```
cd
mkdir -p helm-exercises
cd helm-exercises
```

```
# When ever we encounter error while parsing yaml, we can use comment !!!
helm create testenv
cd testenv/templates
rm -fR *.yaml
rm -fR tests
```

```
nano test.yaml
```

```
# "{{23 -}} < {{- 45}}"
```

```
helm template .. 
```

```
# now with new lines
nano test2.yaml
```

```
# {{23 -}}
newline here

# ohne Umbruch
# {{23 }}
newline: here
```

```
helm template ..
```

## Beispiel wo --debug Sinn macht 

```
# now with new lines
nano test3.yaml
```

```
# ohne Umbruch
# {{23 }}
newline here
```

```
# Fehler weil keine valides Yaml 
helm template ..
# yaml trotzdem anzeigen mit --debug
helm template --debug .. 
```


## Reference:

  * https://pkg.go.dev/text/template#hdr-Text_and_spaces
