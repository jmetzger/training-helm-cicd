# Was ist ein Helm-Chart ? 

## Definition 

  * Ein **Helm Chart** ist ein Paket, das alle nötigen Kubernetes-Ressourcen beschreibt, um eine Anwendung oder einen Dienst bereitzustellen.

## Es enthält: 

- **Templates**: Vorlagen in YAML-Format, die dynamisch Werte einsetzen
- **values.yaml**: Eine Datei mit Konfigurationswerten
- **Chart.yaml**: Metainformationen zum Chart (Name, Version, etc.)
- **Abhängigkeiten**: Optional können andere Charts mit eingebunden werden

---

## Was kann Helm?

- **Installieren** von Anwendungen in Kubernetes (`helm install`)
- **Upgraden** von bestehenden Installationen (`helm upgrade`)
- **Rollbacks** durchführen, falls etwas schiefläuft (`helm rollback`)
- **Anpassen** von Anwendungen durch Konfigurationswerte (`values.yaml`)
- **Veröffentlichen** eigener Charts (z. B. in einem Helm-Repository)

---

## Warum sollte man Helm verwenden?

- **Wiederverwendbarkeit**: Ein Chart kann mehrfach und in unterschiedlichen Umgebungen genutzt werden.
- **Konfigurierbarkeit**: Anpassung an verschiedene Umgebungen wie Entwicklung, Test, Produktion.
- **Automatisierbarkeit**: Ideal für den Einsatz in CI/CD-Pipelines.
- **Große Community**: Viele fertige Charts für beliebte Software wie Prometheus, Grafana, nginx, etc.

---

Wenn du möchtest, kann ich dir ein einfaches Beispiel-Chart zeigen oder einen Helm-Befehl erklären. Sag einfach Bescheid!
