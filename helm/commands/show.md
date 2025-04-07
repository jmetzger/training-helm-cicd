# Show information from a chart 

```
helm show values bitnami/mariadb
helm show values bitnami/mariadb | grep -B 20 -i "image:"
# recommendation -> redirect to file
helm show values bitnami/mariadb > default-values.yaml 
```

```
# Zeigt Chart-Definition, Readme usw. (=alles) an 
helm show all bitnami/mariadb 
```

```
helm show readme
helm show readme bitnami/mariadb
helm show chart bitnami/mariadb
```

```
helm show crds bitnami/mariadb
```

