# Datentypen in values.yaml 


In Helm-Charts (values.yaml) gibt es folgende YAML-Datentypen:

| Typ | Beispiel |
|-----|---------|
| **String** | `name: "hello"` oder `name: hello` |
| **Integer** | `replicas: 3` |
| **Float** | `cpu: 0.5` |
| **Boolean** | `enabled: true` / `false` |
| **Null** | `value: null` oder `value: ~` |
| **List/Array** | `tags: [a, b]` oder Block-Style |
| **Map/Object** | `resources: { limits: {...} }` |
| **Multiline String** | `\|` (literal) oder `>` (folded) |

**Wichtig für Helm:** Typen können in Templates mit Funktionen konvertiert werden:
- `{{ .Values.port | int }}`
- `{{ .Values.enabled | toString }}`
- `{{ .Values.name | quote }}` → erzwingt String-Quotes in YAML-Output

**Typfalle:** `port: 8080` ist ein Integer — im Template ggf. `quote` verwenden wenn ein String erwartet wird.
