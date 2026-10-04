# KU-Buchhaltung

Ein digitales Büro für kleine Dienstleistungsunternehmen: Kunden und Aufträge verwalten, Rechnungen erstellen, Termine planen, Arbeitszeit erfassen und Dokumente wiederfinden.

## Projektstatus

Erster Projektstand: Anforderungen, Verzeichnisstruktur und priorisierter Umsetzungsplan. Es gibt noch keine lauffähige Anwendung. Die technische Plattform wird vor der Implementierung festgelegt.

## Ziel

Den Büroalltag von André Müllers Dienstleistungsunternehmen vereinfachen – vom Kundenauftrag bis zur bezahlten Rechnung. Die Oberfläche soll deutschsprachig, übersichtlich und auf Desktop sowie Smartphone nutzbar sein.

## Geplanter Funktionsumfang

| Bereich | Erste nutzbare Version |
| --- | --- |
| Kunden und Aufträge | Kontaktdaten, Leistungsort, Auftrag, Leistungsbeschreibung und Status |
| Rechnungen | Entwürfe, Positionen, Rechnungsnummern, PDF-Ausgabe, Fälligkeit und Zahlungsstatus |
| Termine | Kalender-/Listenansicht, Kunden- und Auftragsbezug, Beginn und Ende |
| Zeiterfassung | Manuelle Einträge und Start/Stopp mit Pausen, Auftragsbezug und abrechenbare Zeit |
| Dokumente | Upload, Kategorien, Suche sowie Zuordnung zu Kunden, Aufträgen und Rechnungen |
| Übersicht | Offene Rechnungen, nächste Termine und noch nicht abgerechnete Arbeitszeiten |
| Datensicherung | Export, vollständige Sicherung und geprüfte Wiederherstellung |

Typischer Ablauf: Kunde anlegen → Auftrag erfassen → Termin planen → Zeit dokumentieren → Rechnung aus dem Auftrag erstellen → Zahlung erfassen. Dokumente bleiben dem jeweiligen Vorgang zugeordnet.

## Umfang und Grenzen

Die erste Version richtet sich an einen Betrieb mit einem Benutzer. Mehrbenutzerbetrieb, Bankanbindung, automatische Zahlungsabgleiche, OCR, Steuerberater-Schnittstellen und wiederkehrende Abrechnungen folgen gegebenenfalls später.

Die steuerliche Behandlung muss konfigurierbar sein; der Projektname legt keinen Steuerstatus fest. Anforderungen an Rechnungen, E-Rechnungen, Aufbewahrung und nachvollziehbare Änderungen werden vor produktiver Nutzung anhand aktueller offizieller Quellen geprüft. Eine PDF-Ausgabe allein ist kein Nachweis einer gesetzeskonformen E-Rechnung. Dieser Projektstand enthält keine Zusage einer steuerrechtlich geprüften Buchhaltung.

## Verzeichnisstruktur

| Pfad | Aufgabe |
| --- | --- |
| `docs/` | Anforderungen, Architekturentscheidungen und Umsetzungsplan |
| `src/modules/` | Fachmodule für Kunden, Aufträge, Rechnungen, Termine, Zeiten und Dokumente |
| `src/shared/` | Gemeinsame Bausteine, Validierung und technische Schnittstellen |
| `tests/` | Spätere Modul-, Integrations- und Ablaufprüfungen |
| `scripts/` | Entwicklungs-, Export- und Sicherungswerkzeuge |
| `data/` | Lokale Laufzeitdaten; Inhalt wird nicht versioniert |

## Einstieg

1. [Anforderungen](docs/requirements.md) lesen.
2. [Priorisierten Umsetzungsplan](docs/roadmap.md) abarbeiten.
3. [Technische Leitplanken](docs/architecture.md) für die Plattformentscheidung verwenden.

Installation und Startbefehle werden ergänzt, sobald das erste lauffähige Grundgerüst vorhanden ist.

## Umgang mit Daten

Dieses Repository ist öffentlich. Keine echten Kundendaten, Rechnungen, Belege, Zugangsdaten oder Sicherungen einchecken. Beispiele und Tests verwenden ausschließlich erfundene Daten. Laufzeitdaten gehören in einen getrennten, gesicherten Speicher.

## Beiträge

Änderungen mit nachvollziehbarem Ziel und passenden Akzeptanzkriterien umsetzen. Fachliche Berechnungen und wichtige Abläufe prüfen; neue technische Entscheidungen in `docs/` festhalten.
