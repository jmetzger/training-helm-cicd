# Was ist ein Helm-Chart ? 

## Definition 

  * Ein **Helm Chart** ist ein Paket, das alle nötigen Kubernetes-Ressourcen beschreibt, um eine Anwendung oder einen Dienst bereitzustellen.

## Es enthält: 

- **Templates**: Vorlagen in YAML-Format, die dynamisch Werte einsetzen
- **values.yaml**: Eine Datei mit Konfigurationswerten
- **Chart.yaml**: Metainformationen zum Chart (Name, Version, etc.)
- **Abhängigkeiten**: Optional können andere Charts mit eingebunden werden

## Formate / Ort 

  * Verzeichnis z.B. meine-app (und in dem Verzeichnis die bekannte Struktur von oben)
  * tar.gz (Tape-Archive mit gnuzip komprimiert)
  * URL 
