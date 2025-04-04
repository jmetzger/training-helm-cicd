# Templating - spaces

## Explanation 

  * {{- -> trim on left side
  * -}} -> trim on right side / ALSO: new lines 
  * trim tabs, whitespaces a.s.o. (see ref)

## Walkthrough 

```
cd
mkdir helm-exercises
cd helm-exercises
```

```
# When ever we encounter error while parsing yaml, we can use comment !!!
helm create testenv
cd testenv/templates
rm -f *.yaml
```

```
nano test.yaml
```

```
# "{{23 -}} < {{- 45}}"
```

```
helm template .. 
helm template --debug ..
```

## Reference:

  * https://pkg.go.dev/text/template#hdr-Text_and_spaces
