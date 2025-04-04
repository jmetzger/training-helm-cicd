# Ablauf helm install 

Wenn der Befehl `helm install` ausgeführt wird, passiert intern Folgendes:

1. **Chart-Abfrage**:
    * Helm sucht nach dem angegebenen Chart entweder lokal oder im angegebenen Repository.
1. **Chart-Templating**:
    * Helm rendert die Templates im Chart. Dabei werden Variablen (wie in der `values.yaml` definiert) in die Templates eingefügt, wodurch Kubernetes-Ressourcen (z. B. Deployments, Services) erstellt werden.
1. **Kubernetes API**:
   * Die gerenderten Kubernetes-Ressourcen werden an die Kubernetes-API geschickt. Helm verwendet dafür die `kubectl`-API, um Objekte wie Deployments, Services oder ConfigMaps zu erstellen.
1. **Release-Verwaltung**:
   * Helm erstellt ein Release-Objekt und speichert die Chart- und Versionsinformationen in der Helm-Release-Datenbank (z. B. in Kubernetes als Secret oder ConfigMap). Dies ermöglicht eine spätere Verwaltung und Aktualisierung des Releases.
1. **Ausgabe**: Helm gibt den Status des Installationsprozesses aus, einschließlich der erstellten Ressourcen und etwaiger Fehler.

Kurz gesagt: Helm rendert Kubernetes-Ressourcen aus einem Chart und kommuniziert mit der Kubernetes-API, um diese Ressourcen zu erstellen und ein Release zu verwalten.
