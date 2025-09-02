# Helm autogenerate values.yaml 

```
grep -Rho '{{[^}]*\.Values[^}]*}}' charts/mychart/templates/ \
  | sed -E 's/.*\.Values\.([a-zA-Z0-9_.-]+).*/\1/' \
  | sort -u \
  | awk -F. '{
      indent=""
      for (i=1; i<NF; i++) {
        indent=indent"  "
        printf "%s%s:\n", indent, $i
      }
      indent=indent"  "
      printf "%s%s: <TODO>\n", indent, $NF
    }' > values.generated.yaml
```

```
image:
  repository: <TODO>
  tag: <TODO>
service:
  port: <TODO>
  type: <TODO>

```

