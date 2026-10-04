# Anforderungen

## Ausgangspunkt

Ein übersichtliches digitales Büro für ein kleines Dienstleistungsunternehmen mit Rechnungen, Terminen, Zeiterfassung und Dokumentenverwaltung. Beispielhafte Einsätze: Hausservice, Gartenpflege, Büromöbelmontage und Kleintransporte. Die Leistungsarten bleiben frei konfigurierbar.

## Gemeinsame Grundlage

- Ein Benutzer, ein Unternehmen; deutsche Oberfläche und EUR als erste Währung.
- Desktop und Smartphone; gut lesbare Formulare und eindeutige Statusanzeigen.
- Kunden können mehrere Aufträge haben. Termine, Zeiten und Dokumente beziehen sich auf Kunden und optional auf Aufträge.
- Rechnungen enthalten einen festen Stand der Kunden-/Unternehmensdaten und Leistungspositionen; spätere Stammdatenänderungen verändern ausgestellte Rechnungen nicht.
- Suche und Filter für Kunde, Auftrag, Zeitraum und Status.
- Geldbeträge mit exakter Dezimalarithmetik oder ganzzahligen Untereinheiten, keine unkontrollierte Fließkommarechnung.
- Zeitdauer und Rundungsregeln explizit festlegen; Zeitzone Europe/Berlin, Sommerzeitwechsel berücksichtigen.

## Kernbereiche

### Kunden und Aufträge

Kunde mit Name, Anschrift und optionalen Kontaktangaben. Auftrag mit Kunde, Leistungsort, Beschreibung und Status (geplant, aktiv, abgeschlossen, abgebrochen). Pflichtangaben abhängig vom Vorgang validieren.

### Rechnungen

Entwurf mit Leistungszeitraum, Positionen (Beschreibung, Menge, Einheit und Einzelpreis), berechneten Summen, Zahlungsziel und konfigurierter steuerlicher Behandlung. Entwürfe dürfen bearbeitet werden. Beim Ausstellen wird eine eindeutige Nummer vergeben und der Inhalt festgeschrieben. Korrekturen erfolgen nachvollziehbar über eigene Vorgänge. Zahlungseingänge separat erfassen, Teilzahlungen berücksichtigen und Überfälligkeit aus Restbetrag und Fälligkeit ableiten.

PDF herunterladen; automatische E-Mail-Versendung ist zunächst nicht enthalten. Strukturierte E-Rechnungsformate und gesetzliche Pflichtangaben vor Produktivbetrieb gesondert prüfen.

### Termine

Beginn, Ende, Titel, Kunde und optional Auftrag; Listenansicht und Kalender. Ende muss nach Beginn liegen. Überschneidungen anzeigen. Erinnerungen zunächst in der Anwendung; externe Kalender und Benachrichtigungen später.

### Zeiterfassung

Manuelle Erfassung und Timer mit Start, Stopp und Pausen. Auftrag, Leistungsbeschreibung und Kennzeichen „abrechenbar“. Laufender Timer muss einen Neustart überstehen. Negative Dauer, überlappende Einträge und doppelte Abrechnung verhindern. Übernommene Zeiten mit Rechnungsposition verknüpfen; bei verworfenen Entwürfen wieder freigeben.

### Dokumentenverwaltung

Originaldatei, Dateiname, Dateityp, Größe, Erfassungszeit, Kategorie und Zuordnung speichern. Dateien herunterladen und anhand ihrer Metadaten suchen. Dateityp und Größe begrenzen, sichere Speicherpfade verwenden und Dateizugriffe schützen. Originale nicht stillschweigend überschreiben; Änderungen nachvollziehbar machen. OCR und Volltextsuche sind spätere Erweiterungen.

### Übersicht und Sicherung

Offene Beträge, nächste Termine und nicht abgerechnete Zeiten anzeigen. Daten und Dokumente gemeinsam sichern und wiederherstellen. Exportformat dokumentieren. Sicherung außerhalb des Anwendungsspeichers aufbewahren.

## Offene Entscheidungen vor Umsetzung

1. Lokaler Betrieb oder Hosting? Zugriff nur am eigenen Gerät oder auch unterwegs?
2. Technischer Stack und unterstützte Plattformen.
3. Unternehmensstammdaten, steuerliche Behandlung, Nummernkreis und Zahlungsziel.
4. Dokumentengrößen, Speicherbedarf und Sicherungsziel.
5. Anforderungen an produktive Rechnungen und Aufbewahrung anhand aktueller offizieller Quellen.

Diese Punkte sind Entscheidungspunkte, keine bereits bestätigten Nutzeranforderungen.
