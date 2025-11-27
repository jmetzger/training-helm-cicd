# Chart runterladen und evtl entpacken (auch bestimmte version) 

```
cd 
mkdir -p charts-download
cd charts-download
```


```
# Vorher müssen wir den Repo-Eintrag anlegen 
helm repo add bitnami https://charts.bitnami.com/bitnami 

# Lädt die letzte version herunter
helm pull bitnami/mariadb

# Lädt bestimmte chart-version runter 
# helm pull bitnami/mariadb --version 12.1.6
# evtl. entpacken wenn gewünscht
# tar xvf mariadb-12.1.6.tgz

# Schnelle Variante
helm pull bitnami/mariadb --version 12.1.6 --untar
```
