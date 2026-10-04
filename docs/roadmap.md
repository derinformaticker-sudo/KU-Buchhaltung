# Priorisierter Umsetzungsplan

Reihenfolge innerhalb einer Priorität einhalten. Einträge sind geplant, noch nicht umgesetzt.

| Schritt | Priorität | Ergebnis | Abnahme |
| --- | --- | --- | --- |
| 1 | P0 | Betriebsmodell, Stack und erste Oberflächen festlegen | Entscheidung dokumentiert; Ablauf Kunde → Auftrag → Zeit → Rechnung als Entwurf skizziert; lokale und mobile Nutzung geklärt |
| 2 | P0 | Anforderungen an produktive Rechnungen und Datenhaltung prüfen | Aktuelle offizielle Quellen, Prüfdatum und daraus abgeleitete Anforderungen dokumentiert; steuerliche Konfiguration festgelegt |
| 3 | P0 | Lauffähiges Grundgerüst mit Datenbank und Zugriffsschutz | Dokumentierter Start von frischer Installation; Migrationen ausführbar; bei Hosting Anmeldung und geschützte Datei-/Datenzugriffe |
| 4 | P0 | Unternehmensdaten, Kunden und Aufträge | Erstellen, Bearbeiten und Suchen funktionieren; Pflichtangaben validiert; Stammdaten bleiben nach Neustart erhalten |
| 5 | P0 | Rechnungsentwürfe und PDF-Vorschau | Positionen und Summen korrekt; Vorschau mit erfundenen Beispieldaten; noch kein Ausstellen produktiver Rechnungen |
| 6 | P0 | Ausstellen, Nummernkreis, Korrekturen und Zahlungen | Nummern auch bei parallelen Aktionen eindeutig; ausgestellte Inhalte festgeschrieben; Teilzahlungen und Korrekturvorgänge nachvollziehbar |
| 7 | P0 | Sicherung und Wiederherstellung | Datenbank und Dokumente aus einer Sicherung in einer frischen Umgebung wiederhergestellt; Ergebnis geprüft |
| 8 | P1 | Termine mit Kunden-/Auftragsbezug | Kalender und Liste zeigen denselben Stand; ungültige Zeiträume abgewiesen; Überschneidungen sichtbar |
| 9 | P1 | Manuelle Zeit und Timer | Pausen und Neustart berücksichtigt; Zeiten einem Auftrag zugeordnet; doppelte Abrechnung ausgeschlossen |
| 10 | P1 | Dokumentenablage | Upload, Suche, Zuordnung und Download funktionieren; unzulässige Dateien und fremde Zugriffe abgewiesen |
| 11 | P1 | Zeiten in Rechnung übernehmen und Übersicht | Durchgängiger Musterauftrag bis Zahlung; offene Beträge, Termine und freie Zeiten korrekt angezeigt |
| 12 | P1 | Erste Version abnehmen | Kernablauf auf Desktop und Smartphone geprüft; Wiederherstellung erfolgreich; Bedien-/Betriebsanleitung vorhanden |
| 13 | P2 | Erweiterungen auswählen | Bedarf für Angebote, wiederkehrende Leistungen, OCR, Bank-/Steuerberater- und Kalenderanbindung bewertet |

## Meilensteine

- **M1 – Technische Grundlage:** Schritte 1–4 abgeschlossen.
- **M2 – Rechnungen mit Sicherung:** Schritte 5–7 abgeschlossen; Produktivfreigabe erst nach fachlicher Prüfung.
- **M3 – Digitales Büro:** Schritte 8–12 abgeschlossen, alle vier Kernbereiche verbunden.

## Prüfstrategie

Gezielte Prüfungen für Geldberechnung und Rundung, Nummernvergabe, Festschreibung, Teilzahlungen, Zeitdauer inklusive Sommerzeitwechsel, Zugriffsschutz und Dateiannahme. Ein Integrationstest bildet den Musterauftrag vom Kunden bis zur Zahlung ab. Sicherung durch echte Wiederherstellung prüfen.

## Nächste konkrete Aufgabe

Schritt 1: Eine begründete Architekturentscheidung und einen kleinen Oberflächenentwurf erstellen. Anschließend das Grundgerüst implementieren; keine parallelen Insellösungen für die vier Bereiche aufbauen.
