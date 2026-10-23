# ADR 0001: Betriebsmodell, Stack und Datenhaltung

- Datum: 23.10.2026
- Status: Technische Empfehlung für die Umsetzung; keine bestätigte Anbieter-, Budget- oder Produktivfreigabe.
- Grundlage: [Anforderungen](../requirements.md), [Leitplanken](../architecture.md), [Roadmap Schritt 1](../roadmap.md).
- Begleitdokument: [Oberflächenentwurf](../ui/0001-kernablauf.md).
- Ausgangsstand: main bei 6b9dc688552fdcc9f48fd229de8100980b409238; Dokumentation und Modulplätze, keine Anwendung und keine offenen Pull Requests bei Prüfung.

## Kontext und Auswahlkriterien

Ein Benutzer in einem Unternehmen soll Kunde → Auftrag → Zeit → Rechnung auf PC und Smartphone durchgängig bearbeiten. Vorrang haben ein gemeinsamer Datenstand, nachvollziehbare Geld- und Zeitverarbeitung, einfache Wartung und wiederherstellbare Daten. Eine vorhandene Hosting-Instanz oder frühere lokale Lösung wird hierdurch weder als kompatibel vorausgesetzt noch migriert.

## Betriebsmodelle im Vergleich

| Modell | Desktop und Smartphone | Betriebsaufwand und Kostenstruktur | Bewertung |
| --- | --- | --- | --- |
| Nur lokaler PC, Browser auf localhost | PC direkt; Smartphone erreicht dessen localhost nicht. Im LAN sind Serveradresse und laufender PC erforderlich | Keine Hostinggebühr, aber Updates, Sicherung und Erreichbarkeit selbst organisieren | Geeignet bei rein lokaler Nutzung; unterwegs unpraktisch |
| Eigener Server mit VPN | Beide Geräte über VPN; zentraler Datenstand | Server, VPN, Betriebssystem, Datenbank und Sicherungen selbst warten | Kontrollierbar, für den ersten Einzelbetrieb zu viel Administration |
| Zentral gehostete Webanwendung mit verwalteter Datenbank | Browser über HTTPS auf beiden Geräten; PC muss nicht eingeschaltet sein | Laufende Gebühren für App, Datenbank, Speicher und externe Sicherungen; Anbieter betreibt Infrastruktur, App-Verantwortlicher pflegt Anwendung | Empfehlung für Nutzung unterwegs |
| Native Desktop- und Mobil-App mit Offline-Synchronisation | Gute Offline-Nutzung, aber Konflikte zwischen Geräten | Mehrere Clients, Synchronisationsdienst und Versionspflege | Für den ersten Umfang unverhältnismäßig |

Empfohlen: ein zentraler Dienst in einer EU-Region mit verwaltetem PostgreSQL, privatem Dateispeicher und automatisierten externen Sicherungen. EU-Region ist eine Auswahlvorgabe, keine pauschale Datenschutzgarantie. Vor Vertragsabschluss müssen Anbieter, Auftragsverarbeitung, Unterauftragnehmer, Exportmöglichkeiten und Gesamtkosten geprüft werden. Es wird kein Dienst gebucht. Eine konkrete Preisangabe wäre ohne Speicherbedarf und Anbieterentscheidung unbelegt.

Lokal bleibt derselbe Stack für Entwicklung und optionalen Einzelplatzbetrieb nutzbar, vorzugsweise über Docker Compose. Für Smartphone-Zugriff im eigenen LAN ist ein dokumentierter, abgesicherter Zugang erforderlich; keine ungeschützte Portfreigabe ins Internet. Lokale und gehostete Instanzen sind getrennte Datenbestände, keine automatisch synchronisierten Kopien.

## Stackoptionen im Vergleich

Die Bewertung ist eine projektspezifische technische Einschätzung, keine allgemeine Rangliste.

| Option | Vorteile | Aufwand und Grenzen | Entscheidung |
| --- | --- | --- | --- |
| Python/Django, Templates, PostgreSQL | ORM, Migrationen, Formulare, Sitzungen und Admin in einem Framework; Geschäftslogik zentral | Eigene responsive Fachoberfläche und MFA-Integration nötig; Admin allein genügt nicht | Empfohlen |
| TypeScript/Next.js, PostgreSQL, ORM | Sehr flexible interaktive Oberfläche; gemeinsame Sprache für Server und Browser | Authentifizierung, ORM und fachliche Grenzen zusätzlich zusammenstellen; Server-/Client-Zustand und Cache bewusst pflegen | Gute Alternative bei vorhandenem React-Team |
| React-SPA mit separater API und PostgreSQL | Eigenständige API für spätere Clients | Zwei Auslieferungen, API-Verträge und zusätzliche Zustandsverwaltung | Erst bei realem Bedarf für mehrere Clients |
| Desktop-App mit SQLite und lokaler Dateiablage | Einfache lokale Datenhaltung | Smartphone und parallele Nutzung verlangen zusätzliche Server-/Sync-Architektur | Kein primärer Stack für das gewünschte Betriebsmodell |

## Entscheidung

Modularer Monolith: Python 3.13, Django 5.2 LTS mit aktuell gepatchten Versionen, PostgreSQL 17 als konservative unterstützte Basis, Django-Templates und eigenes responsives CSS. Kleine JavaScript-Bausteine für Timeranzeige und dynamische Formulare; HTMX bei belegtem Bedarf, kein eigener SPA-Client. Exakte Paketstände und Container-Digests werden erst im Grundgerüst geprüft und gesperrt.

Eine Auslieferung für Oberfläche und Backend. Produktionsbetrieb mit WSGI-Server hinter HTTPS-Proxy bzw. entsprechender Plattformfunktion; kein Entwicklungsserver. PDF-Erzeugung erhält eine austauschbare Schnittstelle; Bibliotheksauswahl und strukturierte E-Rechnung folgen der fachlichen Prüfung in Schritt 2. Keine Microservices, kein Redis und kein separater Queue-Dienst als Startvoraussetzung.

Django-Apps folgen den vorhandenen Fachmodulen. Der Verzeichnisname time-tracking wird bei Implementierung nachvollziehbar zu time_tracking geändert, weil Python-Module keine Bindestriche verwenden. Projektkonfiguration unter src/config, Querschnittsfunktionen unter src/shared. Views rufen Fachservices auf; diese kapseln Transaktionen. Module schreiben fremde Fachtabellen nur über definierte Services. Admin dient der eingeschränkten Administration, nicht dem Umgehen der Rechnungslogik.

## Datenmodell und Verlässlichkeit

| Objekt | Beziehung und Regel |
| --- | --- |
| Unternehmen, Benutzer | Eine Firma und ein Benutzer zunächst; Eigentumsprüfung an allen Zugriffswegen |
| Kunde | Hat mehrere Aufträge; referenzierte Stammdaten archivieren statt unkontrolliert löschen |
| Auftrag | Gehört zu Kunde; Leistungsort, Beschreibung, Status |
| Termin, Zeiteintrag, Pause | Kundenbezug, optional Auftrag; Rechnung aus Zeit verlangt Auftragszuordnung |
| Rechnung, Position, Zahlung | Kundendaten und Unternehmensdaten als Momentaufnahme; Zahlungen separat; Restbetrag berechnet |
| Zeitzuordnung | Verknüpft Zeiteintrag mit Rechnungsposition; aktive Zuordnung pro Zeiteintrag eindeutig |
| Dokument, Zuordnung | Metadaten in SQL, Originaldatei separat; referenzielle Zuordnung |
| Audit-Ereignis | Wer, wann, Aktion und Objekt; erforderliche Änderung nachvollziehbar, keine Passwörter oder vollständigen Belege im Log |

Geld über Decimal/NUMERIC mit expliziten Mengen-, Preis- und Rundungsregeln; kein JavaScript-Float als verbindliche Berechnung. Zeitpunkte UTC, Anzeige Europe/Berlin; Dauer aus Zeitintervallen abzüglich Pausen. Kalendertermine enthalten die relevante Zeitzone. Timerstart, Pause und Stopp serverseitig speichern; nach Browser-/Serverneustart daraus rekonstruieren. Pro Benutzer maximal ein laufender Timer, Überlappungen auch bei parallelen Anfragen verhindern.

Zeitübernahme sperrt betroffene Einträge in einer Transaktion und reserviert sie für einen Entwurf. Parallele Übernahmen schlagen kontrolliert fehl. Verwerfen des Entwurfs gibt Reservierungen frei; ausgestellte Zuordnungen bleiben erhalten. Anfangs nur vollständige Zeiteinträge übernehmen, keine Teilabrechnung eines Eintrags.

Ausstellen sperrt Entwurf und Nummernkreis in einer Transaktion, validiert erneut, vergibt eine pro Firma/Nummernkreis eindeutige Nummer und schreibt Momentaufnahme, Positionen und Audit-Ereignis fest. Unique Constraints und Idempotenzschlüssel verhindern doppelte Nummern und Doppelklick-Rechnungen. Festgeschriebene Daten über alle Fachschreibwege sperren; Korrekturen sind eigene verknüpfte Vorgänge. Steuerstatus und Nummernkreis sind noch nicht bestätigt.

PDF-Erzeugung ist keine SQL-Transaktion: nach erfolgreicher Festschreibung aus dem festen Stand erzeugen und Status speichern. Bei Fehler bleibt die Rechnung ausgestellt und erhält „PDF ausstehend“; Wiederholung mit gleichem Inhalt und ohne neue Nummer. Später kann ein transaktionaler Outbox-Eintrag einen Worker anstoßen.

## Dateispeicher und Zugriffsschutz

Produktion: privater S3-kompatibler Objektspeicher mit EU-Standort; lokal: privates persistentes Verzeichnis außerhalb von Webroot und Repository. Gemeinsamer Storage-Adapter für Schreiben, Lesen und Prüfen. Keine Dateien in Container-Zwischenspeicher oder SQL-BLOBs als Standard.

Zufällige Objektschlüssel, Originalname nur als Metadatum; Größe, erkannter MIME-Typ, SHA-256 und Speicherstatus in SQL. Startvorschlag: PDF/JPEG/PNG bis 20 MiB, vor produktiver Nutzung anhand echter Anforderungen bestätigen. Erweiterungen und MIME-Inhalt prüfen, ausführbare/aktive Inhalte zurückweisen; Downloads als Attachment mit nosniff. Upload zunächst in Quarantäne; erst nach erfolgreicher Prüfung freigeben. Keine öffentlich auflösbaren Beleg-URLs.

Dateioperationen und SQL können nicht gemeinsam atomar committen: Upload erhält pending-Status, wird nach erfolgreichem Speichern ready; nur ready-Dateien abrufbar. Fehlversuche und verwaiste Objekte per kontrolliertem Abgleich bereinigen. Originale nicht überschreiben, neue Versionen als neue Objekte. Downloads werden durch die Anwendung nach Anmeldung und Objektberechtigung vermittelt.

Anmeldung mit Django-Sitzung, sicher gehashtem Passwort, CSRF-Schutz, Secure-/HttpOnly-/SameSite-Cookies, HTTPS und Login-Limitierung. Keine öffentliche Registrierung; initialer Benutzer wird administrativ angelegt. MFA über gepflegte Integration als Freigabekriterium für öffentlichen Betrieb; Wiederherstellungscodes privat sichern. Alle Listen, Detailansichten, Aktionen, PDFs und Downloads prüfen Berechtigung auf dem Server. Datenbank nur im privaten Netz, separater eingeschränkter App-Benutzer; Secret-Verwaltung außerhalb Git. DEBUG aus, Hosts explizit, private Antworten nicht öffentlich cachen. Sicherungen verschlüsseln, Schlüssel separat sichern.

## Betrieb, Sicherung und Erweiterung

App-Verantwortlicher übernimmt Sicherheitsupdates, Migrationen, Überwachung und Restore-Proben; verwaltete Infrastruktur ersetzt diese Aufgaben nicht. Vor Produktionsstart muss die verantwortliche Person benannt sein. Fehleralarm und Sicherungsalarm dürfen keine Beleginhalte enthalten.

Vorgeschlagene Betriebsziele, noch keine gemessenen Zusagen: tägliche vollständige Sicherung, maximal 24 Stunden Datenverlust (RPO), Wiederanlauf innerhalb eines Arbeitstags (RTO). Sicherungsrotation zunächst 7 tägliche und 4 wöchentliche Stände; gesetzliche Aufbewahrung separat klären, Backuprotation ist kein Archivkonzept.

Für konsistenten Stand kurz Schreibzugriffe und Hintergrundaufgaben anhalten, PostgreSQL-Dump, Originaldateien und Manifest mit Prüfsummen/Schema- und Anwendungsversion sichern. Erst nach vollständig geprüftem Paket wieder freigeben; Sicherung verschlüsselt außerhalb des primären Anbieterkontos speichern. SQL-Dump allein enthält keine Dokumente. Restore in isolierter frischer Umgebung: Migrationstand, Beziehungen, Dateiprüfsummen, Rechnungsstände und Zugriffsschutz prüfen; mindestens vor Produktivfreigabe und nach wesentlichen Speicheränderungen.

Spätere Erweiterungen: Termine und Dokumente bleiben eigene Module; Angebote, OCR, Bank-/Steuerberaterexport, E-Mail und E-Rechnung über Fachservices/Adapter anbinden. API erst für konkreten externen Client. Worker erst bei langsamen Jobs. Mehrbenutzerbetrieb verlangt eigene Rollen-/Berechtigungsentscheidung; vorhandene Eigentumsfelder machen daraus noch kein Mandantensystem. Zervant-Import und Übernahme vorhandener Daten benötigen Bestandsanalyse, Testimport und Duplikatstrategie.

## Folgen und nächste Schritte

Vorteile: ein Datenstand, eine Oberfläche, etablierte SQL-Transaktionen und überschaubare Auslieferung. Nachteile: Internet und laufende Hostingkosten; Verantwortung für Anwendungspflege; weniger Offline-Komfort. Bei Netzausfall keine neuen Buchungen oder Timeraktionen; ein bereits bestätigter Timer läuft als gespeicherter Zustand weiter. Manuelle Nachtragung nach Wiederverbindung möglich. Keine Erfolgsmeldung vor Serverbestätigung.

Schritt 1 wird mit diesem Dokument und dem UI-Entwurf zur Abnahme vorgelegt. Schritt 2 bleibt offen: aktuelle offizielle Rechnungs-/Aufbewahrungsanforderungen und steuerliche Einstellungen prüfen. Danach Schritt 3 mit Migrationen, Login und frischem dokumentiertem Start. Schritt 1 allein bedeutet weder lauffähige Anwendung noch Produktivfreigabe.

Abnahme des Grundgerüsts: Persistenz nach Neustart, verweigerte anonyme/fremde Zugriffe, geschützter Testdownload, parallele Zeitübernahme und Nummernvergabe, idempotente Wiederholung, Timerneustart, Restore und Desktop-/Smartphone-Kernablauf. Produktive Rechnungen erst nach den fachlichen und technischen Freigabeschritten der Roadmap.

## Quellen

Geprüft am 23.10.2026; offizielle technische Dokumentation. Die Empfehlung und Gewichtung sind eigene Ableitungen für dieses Projekt.

- [Django 5.2 LTS und Python-Kompatibilität](https://docs.djangoproject.com/en/5.2/releases/5.2/) – langfristige Wartungsbasis.
- [Django-Transaktionen](https://docs.djangoproject.com/en/5.2/topics/db/transactions/) – atomare Fachoperationen; externe Dateioperationen gesondert behandeln.
- [Deployment-Checkliste](https://docs.djangoproject.com/en/5.2/howto/deployment/checklist/) – Produktionskonfiguration, HTTPS und geschützte Uploads.
- [PostgreSQL SQL-Dump](https://www.postgresql.org/docs/current/backup-dump.html) – Datenbank sichern/wiederherstellen; Dateisicherung zusätzlich nötig.
- [Next.js Self-Hosting](https://nextjs.org/docs/app/guides/self-hosting) – Betriebs- und Cacheaspekte der Alternative.
- [HTMX-Dokumentation](https://htmx.org/docs/) – optionale schrittweise Anreicherung von HTML-Formularen.
