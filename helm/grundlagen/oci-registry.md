# Helm + OCI: Warum das sinnvoll ist

## Das Problem vorher

Helm hatte ein eigenes Registry-Protokoll: ein Chart Repository war ein einfacher HTTP-Server mit einer `index.yaml`-Datei. Das bedeutete:

- Separates Tool/Infra noetig (ChartMuseum, GitHub Pages, etc.)
- Kein Content-Addressable Storage (dazu gleich mehr)
- Keine standardisierten Auth-Mechanismen
- Zweite Registry-Infrastruktur neben der Container-Registry betreiben

## Die Loesung: OCI als generisches Artifact-Format

OCI (Open Container Initiative) definiert nicht nur Container-Images, sondern ein generisches Artifact-Format.
Ein Helm Chart ist letztlich nur ein `tar.gz` mit Metadaten - das passt perfekt ins OCI-Manifest-Modell:

```
OCI Manifest
├── config (mediaType: application/vnd.cncf.helm.config.v1+json)
└── layers
    └── chart.tar.gz (mediaType: application/vnd.cncf.helm.chart.content.v1.tar+gzip)
```

## Warum haben sie das gemacht?

- **Eine Registry fuer alles** - Container-Images und Helm Charts in derselben Registry (ECR, GCR, GHCR, Harbor, etc.)
- **Auth gratis** - `docker login` / OIDC-Flows funktionieren sofort
- **Content-Addressable** - Immutable Digests, kein Index-Drift (dazu gleich mehr)
- **Existing Tooling** - Cosign fuer Signing, ORAS fuer generische Artifacts, Vulnerability-Scanner
- **Supply Chain** - SBOMs, Attestations und Signaturen direkt am Artifact referenzierbar

## Was bedeutet Content-Addressable?

Content-Addressable bedeutet: **der Name eines Artifacts ist sein Inhalt** (als Hash).

Konkret: Statt "ich lade Datei `mychart-1.0.0.tgz` vom Server X" heisst es:
"ich lade das Artifact mit SHA256-Digest `sha256:abc123...`" - egal von welchem Server.

```
# Klassisch (URL-basiert) - mutabel, kann sich aendern:
https://charts.example.com/mychart-1.0.0.tgz
  --> heute: Version A
  --> morgen: jemand pushed neu, gleicher Name, anderer Inhalt

# Content-Addressable (Digest-basiert) - immutabel:
sha256:3f4a2b1c8d...
  --> immer: exakt dieser Inhalt, kein anderer
```

Der SHA256-Hash wird aus dem Inhalt berechnet. Wenn sich auch nur ein Byte aendert, ist der Hash komplett anders.

| Eigenschaft | Erklaerung |
|-------------|------------|
| **Immutabel** | Ein Digest referenziert immer exakt denselben Inhalt |
| **Verifizierbar** | Der Client kann den Hash selbst nachrechnen und pruefen |
| **Kein Drift** | Kein "index.yaml veraltet" oder "falsches Chart geladen" |
| **Caching** | Identischer Digest = bereits gecacht, kein Re-Download |

In OCI-Registries gilt: **Tags sind mutabel** (`:latest`, `:1.0.0` koennen ueberschrieben werden), **Digests sind immutabel**. Fuer reproduzierbare Builds pinnt man deshalb auf Digests:

```
# Tag - kann sich aendern:
helm install myapp oci://registry.example.com/charts/mychart --version 1.0.0

# Digest-Pin - unveraenderlich:
helm install myapp oci://registry.example.com/charts/mychart@sha256:3f4a2b1c8d...
```

## Praktisch

```
# Push
helm push mychart-1.0.0.tgz oci://registry.example.com/charts

# Pull/Install
helm install myapp oci://registry.example.com/charts/mychart --version 1.0.0
```

Kein `helm repo add` mehr noetig - direkte URL wie bei Container-Images.
