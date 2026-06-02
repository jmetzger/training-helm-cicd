# YAML vs XML – Wann was verwenden?

## Auf einen Blick

| Merkmal | YAML | XML |
|---|---|---|
| Lesbarkeit | Sehr hoch – minimale Syntax | Mittel – viel "Rauschen" durch Tags |
| Schreibaufwand | Gering | Hoch (öffnende + schließende Tags) |
| Kommentare | Ja (`#`) | Nein (nur `<!-- -->`) |
| Schemas / Validierung | Eingeschränkt (JSON Schema) | Stark (XSD, DTD) |
| Namespaces | Nicht vorhanden | Eingebaut |
| Verbreitung DevOps / Cloud | De-facto-Standard | Selten |
| Verbreitung Enterprise / Legacy | Selten | Weit verbreitet |

---

## YAML – Vorteile

- **Lesbar wie Prosa** – Einrückung statt Tags
- **Kompakt** – weniger Zeichen für dieselbe Information
- **Kommentare** direkt im File möglich
- Standard für **Kubernetes, Helm, GitHub Actions, Docker Compose, Ansible**

```yaml
# Beispiel: Kubernetes Deployment
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-app
spec:
  replicas: 2
```

---

## XML – Vorteile

- **Strenge Validierung** via XSD-Schema – ideal für vertragliche Datenformate
- **Namespaces** ermöglichen Kombination verschiedener Standards in einem Dokument
- **Reife Toolchain**: XSLT, XPath, XQuery für Transformationen und Abfragen
- Weit verbreitet in **Enterprise-Systemen**, SOAP, Maven/Ant, Java-Ökosystem

```xml
<!-- Beispiel: Maven pom.xml -->
<project>
  <groupId>com.example</groupId>
  <artifactId>my-app</artifactId>
  <version>1.0</version>
</project>
```

---

## Entscheidungshilfe

| Situation | Empfehlung |
|---|---|
| Kubernetes / Helm / CI-CD | **YAML** |
| Konfigurationsdateien für DevOps-Tools | **YAML** |
| Enterprise-Integration (SOAP, ESB) | **XML** |
| Datenaustausch mit strengem Vertragsformat | **XML** (oder JSON) |
| Build-System (Maven, Ant) | **XML** (vorgegeben) |
| Menschlich lesbare Config, schnell editierbar | **YAML** |

---

## Fazit

> **YAML** ist die erste Wahl in modernen Cloud- und DevOps-Umgebungen.  
> **XML** bleibt relevant, wo strenge Validierung, Namespaces oder Legacy-Systeme gefordert sind.
