# Technische Leitplanken

## Status

Noch kein Stack beschlossen. Dies ist eine Orientierung für die erste Architekturentscheidung, keine bereits implementierte Architektur.

## Vorschlag

Eine modulare Anwendung mit gemeinsamer relationaler Datenbank und getrenntem Dateispeicher. Eine responsiv bedienbare Oberfläche ist für Desktop und Smartphone sinnvoll. Backend und Oberfläche können zunächst gemeinsam ausgeliefert werden; Microservices sind für den ersten Umfang nicht erforderlich.

Die Auswahl von Framework, Datenbank und Hosting erfolgt anhand des Betriebsmodells. Vorher keine Paketversionen oder Startbefehle als bereits vorhanden dokumentieren.

## Verantwortlichkeiten

| Modul | Verantwortung |
| --- | --- |
| customers | Kundenstammdaten |
| orders | Aufträge und Leistungsorte |
| invoices | Entwürfe, Positionen, Nummern, Festschreibung, Korrekturvorgänge und Zahlungen |
| appointments | Termine und Überschneidungen |
| time-tracking | Arbeitszeiten, Pausen, Timer und Abrechnungszuordnung |
| documents | Dateimetadaten, Zuordnung und geschützter Dateispeicher |
| shared | Validierung, Geld-/Zeittypen, Datenzugriff, Zugriffsschutz und Schnittstellen |

Fachmodule greifen aufeinander über klar benannte Schnittstellen zu. Rechnungen übernehmen Stammdaten als Momentaufnahme und verknüpfen abgerechnete Zeiten. Dateien werden außerhalb des öffentlichen Quellcodes gespeichert.

## Geplante Datenobjekte

Unternehmen, Kunde, Auftrag, Termin, Zeiteintrag mit Pausen, Rechnungsentwurf/Rechnung mit Positionen, Zahlung, Dokument mit Zuordnungen und Änderungsprotokoll. Konkrete Tabellen, Beziehungen und Migrationen folgen nach der Stackentscheidung.

## Verlässlichkeit und Betrieb

- Datenbanktransaktionen für Nummernvergabe, Ausstellen und Zeitübernahme.
- Keine automatischen Änderungen an ausgestellten Rechnungen.
- Validierung sowohl an der Oberfläche als auch beim Speichern.
- Bei Hosting: authentifizierte Zugriffe, verschlüsselte Verbindung und geschützte Downloads.
- Konfiguration und Geheimnisse außerhalb der Versionsverwaltung.
- Protokolle ohne unnötige personenbezogene Inhalte.
- Versionierte Datenbankmigrationen; Export und Sicherung mit dokumentiertem Format.
- Sicherung umfasst Daten und zugehörige Originaldateien; Wiederherstellung gehört zur Abnahme.

## Entscheidungen dokumentieren

Für wesentliche Entscheidungen in docs/decisions/ eine Datei mit Kontext, Entscheidung, Alternativen und Folgen anlegen. Die erste betrifft Betriebsmodell, Stack und Datenbank.
