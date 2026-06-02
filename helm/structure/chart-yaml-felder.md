# Chart.yaml - Alle Felder im Ueberblick

Offizielle Doku: https://helm.sh/docs/topics/charts/ → Abschnitt "The Chart.yaml File"

## Vollstaendiges Beispiel (Helm 3+, apiVersion: v2)

```
apiVersion: v2          # (required) immer "v2" fuer Helm 3+
name: mychart           # (required)
version: 1.0.0          # (required) SemVer 2
kubeVersion: ">=1.26"   # (optional) kompatible K8s-Versionen
description: "..."      # (optional)
type: application       # (optional) application | library
keywords:
  - nginx
home: https://...       # (optional) Projekt-URL
sources:
  - https://github.com/...
dependencies:
  - name: redis
    version: "17.x.x"
    repository: "https://charts.bitnami.com/bitnami"
    condition: redis.enabled   # optional
    tags: [cache]              # optional
    import-values: []          # optional
    alias: myredis             # optional
maintainers:
  - name: Jochen Metzger
    email: j@example.com
    url: https://...
icon: https://.../icon.png
appVersion: "7.2.1"     # (optional) App-Version, kein SemVer erzwungen
deprecated: false       # (optional)
annotations:
  example.com/custom: "wert"   # custom metadata hierher
```

## Feldbeschreibungen

| Feld | Pflicht | Beschreibung |
|------|---------|--------------|
| `apiVersion` | ja | Immer `v2` fuer Helm 3+ |
| `name` | ja | Name des Charts |
| `version` | ja | Chart-Version nach SemVer 2 |
| `kubeVersion` | nein | Kompatible Kubernetes-Versionen als SemVer-Range |
| `description` | nein | Kurzbeschreibung des Charts |
| `type` | nein | `application` (Standard) oder `library` |
| `keywords` | nein | Suchbegriffe fuer Chart-Repositories |
| `home` | nein | URL zur Projektseite |
| `sources` | nein | URLs zu den Quellcode-Repositories |
| `dependencies` | nein | Abhaengige Charts (fruehher `requirements.yaml`) |
| `maintainers` | nein | Liste der Maintainer mit name/email/url |
| `icon` | nein | URL zu einem Icon (PNG/SVG) |
| `appVersion` | nein | Version der enthaltenen Anwendung (kein SemVer erzwungen) |
| `deprecated` | nein | Chart als veraltet markieren |
| `annotations` | nein | Beliebige Key-Value-Metadaten |

## Wichtiger Hinweis: annotations statt freier Felder

Seit Helm v3.3.2 sind zusaetzliche (nicht-standardisierte) Felder nicht mehr erlaubt.
Custom Metadata gehoert ausschliesslich in `annotations`:

```
# FALSCH (wird abgelehnt seit v3.3.2):
myCustomField: "wert"

# RICHTIG:
annotations:
  example.com/myCustomField: "wert"
```

## type: application vs. library

| Typ | Verwendung |
|-----|------------|
| `application` | Normales Chart, das direkt installiert werden kann |
| `library` | Enthaelt nur Templates/Helpers, kann nicht direkt installiert werden |

Library-Charts werden als Dependency eingebunden und stellen gemeinsame Template-Logik bereit.

## version vs. appVersion

```
version: 1.0.0       # Version des Helm Charts selbst
appVersion: "7.2.1"  # Version der Anwendung im Chart (z.B. nginx 1.27.0)
```

Beide koennen unabhaengig voneinander erhoehen werden.
Bei `appVersion` sind auch Strings wie `"latest"` oder `"main"` erlaubt.
