## Wann Anführungszeichen in yaml und api-resourcen (kubernetes, ocp)

In YAML sind Anführungszeichen *optional*, aber es gibt einige klare Regeln, **wann du sie brauchst** und **wann du sie besser weglässt**. Ich erkläre es dir allgemein und dann mit Blick auf Kubernetes-Ressourcen.

---

## ✅ **Wann du in YAML Anführungszeichen verwenden solltest**

| Fall                                                                                                 | Beispiel                     | Erklärung                                                                       |
| ---------------------------------------------------------------------------------------------------- | ---------------------------- | ------------------------------------------------------------------------------- |
| **Sonderzeichen** wie `:`, `#`, `@`, `{`, `}`                                                        | `"abc:123"`                  | Ohne Quotes würde `:` als Trennzeichen interpretiert.                           |
| **Strings, die mit `~`, `null`, `yes`, `no`, `on`, `off`, `true`, `false` beginnen**                 | `"no"`                       | YAML interpretiert unquoted `no` als `false`.                                   |
| **Strings mit Leerzeichen oder Zeilenumbrüchen**                                                     | `"Hello World"`              | Ohne Quotes wäre das ein Syntaxfehler.                                          |
| **Zahlen, die nicht als Zahl interpretiert werden sollen** (z. B. Postleitzahlen mit führender Null) | `"01234"`                    | Ohne Quotes würde `01234` als Oktalzahl gelesen.                                |
| **Strings mit Unicode oder Escape-Sequenzen**                                                        | `"Line\nBreak"`              | Nur mit doppelten Quotes wird `\n` als Linebreak erkannt.                       |
| **Variablen oder Helm-Templates** in Helm Charts                                                     | `value: "{{ .Values.foo }}"` | Hier brauchst du doppelte Quotes, damit die Klammern als String erkannt werden. |

---

## ❌ **Wann du in YAML keine Anführungszeichen brauchst**

| Fall                                            | Beispiel           | Erklärung                                 |
| ----------------------------------------------- | ------------------ | ----------------------------------------- |
| **Einfache Strings ohne Sonderzeichen**         | `name: nginx`      | Klarer, einfacher Name – kein Risiko.     |
| **Zahlen, die wirklich Zahlen sind**            | `replicas: 3`      | Wird korrekt als Zahl erkannt.            |
| **Booleans, die wirklich booleans sein sollen** | `enabled: true`    | Wird korrekt als boolescher Wert erkannt. |
| **Listen und Dictionaries**                     | `ports: [80, 443]` | Quotes wären unnötig.                     |

---

## 📦 Bei Kubernetes-Ressourcen (manifeste YAMLs)

In Kubernetes-YAML-Dateien wie `Deployment`, `Service`, `ConfigMap`, usw. gelten die gleichen YAML-Regeln, aber einige typische Beispiele:

| Feld                 | Beispiel                              | Mit/ohne Quotes                                                    |
| -------------------- | ------------------------------------- | ------------------------------------------------------------------ |
| `kind`, `apiVersion` | `kind: Deployment`                    | Ohne Quotes                                                        |
| Container-Befehle    | `command: ["sh", "-c", "echo Hello"]` | Strings in Arrays **mit** Quotes                                   |
| `env`-Variablen      | `value: "123"`                        | Quotes empfehlenswert, da es sonst als Zahl gedeutet werden könnte |
| `args` mit Template  | `args: ["--url={{ .Values.url }}"]`   | Quotes zwingend wegen Helm-Template                                |

---

## 💡 Faustregel

* 🔹 **Immer Quotes**, wenn es Zweifel geben könnte.
* 🔹 **Keine Quotes nötig**, wenn es ein klarer einfacher Wert ist (String ohne Sonderzeichen, Zahl, Bool).
* 🔹 **Bei Helm immer vorsichtig sein** – lieber doppelte Quotes `"{{ }}"` statt einfache `'{{ }}'` oder ganz ohne.

---

Wenn du willst, zeige ich dir gerne ein konkretes Kubernetes-Beispiel mit und ohne Quotes.
