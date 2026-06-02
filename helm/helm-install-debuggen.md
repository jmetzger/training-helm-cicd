## Situationsbeschreibung

```
o Wir haben ein Chart installiert, hat geklappt (aber pods liefen nicht)
o Wir haben es deinstalliert
o Wir haben es nochmal installiert (hat nicht geklappt weil alte Objekte im Weg waren, die nicht gelöscht worden sind 

o UND: kubectl delete ns <namespace-der-applikation> -> IST KEINE OPTIOM
```

## Was könnte nicht gelöscht worden sein

  * PVC (Persistent Volume Claims)
  * RBAC (Service Accounts)
  * CRD's

## Wie debuggen ? 

### Möglichkeit 1: helm chart runterladen und prüfen was wurde installiert 

```
# So oder klonen von gitlab  
cd; helm pull oci://registry-1.docker.io/cloudpirates/mariadb --version 0.16.4 --untar
# Was würde das installieren
helm template mariadb | grep -i -A 4 kind

# Dann überprüfen, ob alle diese Objekt auch gelöscht wurden, ansonsten händisch löschen
```

### Möglichkeit 2: Informationen zur Installation sind noch in ocp/kubernetes vorhanden

```
# Testweise installiert, damit das machen  können
helm install my-mariadb oci://registry-1.docker.io/cloudpirates/mariadb --version 0.16.4
```

```
helm get manifest my-mariadb | grep -i -A 4 kind
```


