# Kalibrierung des Moodle-Prüfstandards

Recherche: **05.10.2026**. Primärquellen: Moodle Developer Resources, MoodleHQ Plugin CI und drei Maintainer-Repositories. Dokumentation und Testcode gelesen; die Referenztests wurden hier nicht ausgeführt. Empfehlungen unten sind unser Vorschlag, keine Moodle-Zertifizierung.

## Ergebnis

Der bisherige Entwurf kombiniert sinnvolle Moodle-Schutzanforderungen mit zusätzlichen Anforderungen an Scanner und Freigabenachweise. Seine zehn gleich gewichteten Bereiche sind **unsere Zusammenstellung**, keine offizielle Moodle-Punkteskala. Besonders der automatische Abzug für fehlende dedizierte Scanner, nicht nachgewiesene Release-ZIP-Prüfung und noch nicht untersuchte Plattformregeln macht den Wert schlecht als Aussage über Sicherheit oder übliche Moodle-Testpraxis nutzbar.

**Empfehlung:** vier Basisbereiche, zusätzliche Prüfmodule nur bei passenden Funktionen und eine schlanke Freigabecheckliste. Schutzwirkung, Testautomatisierung und zusätzliche Werkzeuge getrennt darstellen. Die bisherigen 35 % des AI Chat bleiben eine historische Bewertung des alten Plans; sie ergeben weder „35 % sicher“ noch eine neue höhere Zahl ohne erneute fallweise Prüfung.

## Was Moodle tatsächlich vorgibt

| Thema | Einordnung der Primärquelle | Folgerung für unseren Standard |
| --- | --- | --- |
| Login, Kurs-/Modulkontext, Capabilities, Gruppen | Sicherheitsrichtlinien zur Implementierung | Muss an den relevanten Einstiegspunkten geprüft sein; eigene Rechte- und Fremdobjektfälle testen |
| Typisierte Eingaben, sichere Ausgabe, gebundenes SQL | Sicherheitsrichtlinien zur Implementierung; problematische Eingaben sollen getestet werden | Plugin-eigene Eingabe-/Ausgabepfade prüfen; keinen universellen SQL-Angriffskatalog unabhängig vom Code verlangen |
| CSRF und HTTP-Methode | Form API prüft Sesskey und Methode automatisch; eigene Aktionslinks müssen passend abgesichert sein | Review der tatsächlich genutzten API zählt; eigene Endpunkte/Umgehungsmöglichkeiten gezielt negativ testen |
| Privacy API | Passender Provider für alle Plugins; Datenmodell bestimmt Metadaten-, Export- und Löschpflichten | Auch ohne persönliche Speicherung den zulässigen null_provider prüfen; eigene Speicherung/Weitergabe aktiviert vertiefte Fälle |
| PHPUnit und Behat | Empfohlene Entwicklungswerkzeuge; Core-Prozess verlangt automatisierte Abdeckung neuer Features | Core-Integrationspflicht nicht pauschal als Marketplace-Pflicht für jedes Drittplugin darstellen |
| Plugin CI und Prechecks | Syntax, Struktur, Style, Templates und Testausführung; Prechecks prüfen normalerweise keine tatsächliche Sicherheit | Grundausstattung, aber kein Ersatz für Rechteprüfung und eigene sicherheitsrelevante Tests |

Quellen: [Security](https://moodledev.io/general/development/policies/security), [CSRF](https://moodledev.io/general/development/policies/security/crosssite-request-forgery), [Privacy API](https://moodledev.io/docs/5.3/apis/subsystems/privacy), [Core-Entwicklungsprozess](https://moodledev.io/general/development/process), [Prechecks](https://moodledev.io/general/community/plugincontribution/codeprechecks). Die [Peer-Review-Checkliste](https://moodledev.io/general/development/process/peer-review) verbindet Sicherheitsreview mit Tests und erlaubt ausdrücklich nicht anwendbare Punkte. Der [Plugin-CI-Einstieg](https://moodlehq.github.io/moodle-plugin-ci/) empfiehlt, nicht benötigte Schritte aus der Vorlage zu entfernen.

Die [MoodleHQ-CI-Vorlage](https://github.com/moodlehq/moodle-plugin-ci/blob/main/gha.dist.yml) enthält Qualitätsprüfungen, Installation, Savepoints, PHPUnit und Behat; PHP Mess Detector ist dort nicht blockierend und Code-Coverage standardmäßig ausgeschaltet. Dedizierte SAST-, Secret- oder Dependency-Scanner sind dort keine eigenständigen Pflichtschritte. Das ist ein Befund zur Vorlage, kein Beweis, dass entsprechende Prüfungen unnötig sind.

## Stichprobe etablierter Plugins

Gezielt unterschiedliche Funktionen gewählt: personenbezogene Anwesenheitsdaten, Checklist mit Gruppen/Kommentaren und externes Dateisystem. Die Stichprobe zeigt praktikable Muster, keinen repräsentativen Marktstandard und keine Sicherheitsfreigabe der Projekte.

| Referenz und gelesener Stand | Tatsächliche Prüfungen | Für uns relevantes Beispiel |
| --- | --- | --- |
| [Attendance, Dan Marsden](https://github.com/danmarsden/moodle-mod_attendance/tree/786991c95c0e02ba8893d9d0fc60ca157f2a8bf3), MOODLE_503_STABLE | Moodle Plugin CI: Installation, PHP-Lint, PHPCS/PHPDoc, Validate, Savepoints, Grunt, PHPUnit, eigene Behat-Szenarien; PHP 8.3/8.4 und PostgreSQL/MariaDB gegen main | Produktive External-Funktionen verweigern Schülern das Anlegen von Sessions und das Abrufen von Sessiondetails; getrennte Gruppen verlangen eine gültige Gruppenzuordnung |
| [Checklist, Davo Smith](https://github.com/davosmith/moodle-checklist/tree/fc857ce427dc21025311de20309ec2547a85fe94), master | Moodle Plugin CI mit Moodle-/PHP-/DB-Matrix, Qualitätsprüfungen, PHPUnit und vielen eigenen Behat-Szenarien | Gruppenszenario prüft ausdrücklich unsichtbare fremde Schüler und eingeschränkten Zugriff eines Lehrers; Privacy-Tests prüfen Metadaten, Export und mehrere Löschvarianten |
| [ObjectFS, Catalyst](https://github.com/catalyst/moodle-tool_objectfs/tree/e2ac620651c5ecdec003f1133a3a38e87195d4c9), MOODLE_404_STABLE | Delegation an Catalyst-Moodle-Workflow; dort Qualität, Savepoints, PHP-Kompatibilität, eigene PHPUnit-Suite und Core-Privacy-Compliance-Test, Behat nach ermittelter Matrix | Datenerhalt: lokale Datei ohne externe Kopie wird nicht gelöscht, deaktivierte Löschung bewahrt Kopien; Tokenablauf-Check unterscheidet OK, Warnung und abgelaufen |

Direkte Quellen:

- Attendance: [Workflow](https://github.com/danmarsden/moodle-mod_attendance/blob/786991c95c0e02ba8893d9d0fc60ca157f2a8bf3/.github/workflows/ci.yml), [Anlegen verweigern/Gruppenvalidierung](https://github.com/danmarsden/moodle-mod_attendance/blob/786991c95c0e02ba8893d9d0fc60ca157f2a8bf3/tests/external/add_session_test.php), [Lesen verweigern](https://github.com/danmarsden/moodle-mod_attendance/blob/786991c95c0e02ba8893d9d0fc60ca157f2a8bf3/tests/external/get_session_test.php).
- Checklist: [Workflow](https://github.com/davosmith/moodle-checklist/blob/fc857ce427dc21025311de20309ec2547a85fe94/.github/workflows/moodle-ci.yml), [Gruppen](https://github.com/davosmith/moodle-checklist/blob/fc857ce427dc21025311de20309ec2547a85fe94/tests/behat/groups.feature), [Privacy](https://github.com/davosmith/moodle-checklist/blob/fc857ce427dc21025311de20309ec2547a85fe94/tests/privacy_provider_test.php).
- ObjectFS: [Delegation](https://github.com/catalyst/moodle-tool_objectfs/blob/e2ac620651c5ecdec003f1133a3a38e87195d4c9/.github/workflows/ci.yml), [gelesener gemeinsamer Workflow](https://github.com/catalyst/catalyst-moodle-workflows/blob/48c522dc02dbf280d91cd920a4d6d613051008c7/.github/workflows/ci.yml), [Datenerhalt](https://github.com/catalyst/moodle-tool_objectfs/blob/e2ac620651c5ecdec003f1133a3a38e87195d4c9/tests/local/object_manipulator/deleter_test.php), [Tokenablauf-Meldung](https://github.com/catalyst/moodle-tool_objectfs/blob/e2ac620651c5ecdec003f1133a3a38e87195d4c9/tests/token_expiry_test.php).

Wichtige Grenzen: Ein Test der Tokenablauf-Meldung beweist keine Zugriffsverweigerung mit abgelaufenem Token. ObjectFS referenziert den gemeinsamen Workflow über main, daher ist dessen gelesener Stand ein Recherche-Snapshot und nicht zwingend der Stand früherer Läufe. Die hier gelesenen Workflows enthalten keine dedizierte Scannerkette; interne Prüfungen und GitHub-Plattformfunktionen wurden nicht erhoben.

## Vorschlag für eine schlanke Baseline

Diese Auswahl ist unsere Kalibrierung aus Quellen und Stichprobe.

| Basisbereich | Kleiner, ausreichender Ausgangspunkt |
| --- | --- |
| B1 Qualität und Kompatibilität | Moodle Plugin CI mit benötigten Qualitätschecks und eigenen Tests; deklarierte/eingesetzte Moodle-Version berücksichtigen; DB-Auswahl begründen. Installation und bei Schemaänderungen Upgrade prüfen |
| B2 Anmeldung und Berechtigungen | Einstiegspunkte und Kontexte dokumentieren; unberechtigte Rolle sowie fremde Kurs-/Nutzer-/Objekt-ID an eigener Schutzgrenze prüfen; zulässigen Fall daneben |
| B3 Zustandsänderungen | Form-/AJAX-/Webservice-Schutz im Review nachvollziehen; bei eigener Request-Verarbeitung fehlende/ungültige Sesskey oder Token negativ prüfen; API-gerechte Methode und unveränderte Daten bei Ablehnung |
| B4 Eingaben und Ausgaben | Typisierung, DML und Ausgabe-API prüfen; problematische Zeichen und manipulierte externe Inhalte an eigenen Verarbeitungspfaden; eigenes HTML/Markdown gezielt auf XSS prüfen |

Nicht jedes Plugin hat Schreibaktionen, HTML-Ausgabe oder eigene SQL-Abfragen. Entsprechend entfallen die jeweiligen Fälle mit dokumentierter Begründung. Standard-Moodle-Sesskey, HTML Purifier oder DML nicht umfassend neu testen; prüfen, dass das Plugin sie korrekt einbindet und keine eigene Umgehung schafft. Das spart Tests der Plattform und konzentriert Arbeit auf unsere Fehlerquellen.

## Zusätzliche Module nach Funktion

| Auslöser | Passende Fälle |
| --- | --- |
| Eigene Dateien, Upload/Download, pluginfile | Kontext/Eigentümer, fremde Datei-ID, eigene Pfadverarbeitung, zulässige Dateitypen und Größen |
| Externe Requests mit beeinflussten Zielen | Erlaubte Ziele, interne Ziele/SSRF, relevante Redirects, TLS und Zeit-/Größenlimits; kein vollständiger DNS-Labortest ohne entsprechendes Risiko |
| Personenbezogene Speicherung/Weitergabe | Passender Privacy-Provider, eigener Export/Löschung, Fremddaten bleiben unzugänglich; Datenempfänger und Aufbewahrung beschreiben |
| OAuth, Tokens oder externes Tool | Eigene Identitäts-/Tokenprüfung, Ablauf/Widerruf, Rechtebindung; CSRF/CORS entsprechend Authentifizierungsmodell |
| KI/LLM | Unvertrauenswürdige Modellantworten, erlaubte Daten für Prompt/Antwort, Kosten-/Mengenlimits; aktionsausführende KI benötigt erneute Rechteprüfung |
| Eingebundene Drittbibliotheken | Relevante Bibliotheken/Versionen inventarisieren, bekannte Schwachstellen prüfen; Auditwerkzeug falls passend, kein leerer Paket-Audit als Sicherheitsnachweis |
| Backup/Restore, Migration, Löschung | Datenerhalt, Berechtigungen nach Restore, eigener Upgrade-/Löschpfad |

SAST und Secret-Scanning bleiben sinnvolle **ergänzende Werkzeuge**, besonders im gemeinsamen Workflow. Ihre Installation erzeugt keine eigene Schutzwirkung und ersetzt kein Review. Keine Scanner-Marke, keine Fixtures für unveränderte Fremdregeln und kein separater Scanner-Score als allgemeine Moodle-Pflicht. Eigene Semgrep-Regeln hingegen sollten auf Erkennung und Fehlalarme getestet werden.

## Nachweise und Fortschrittszahl

- Pro Projekt zuerst anwendbare, konkret benannte Fälle festlegen. Gemeinsame Moodle-Schutzmechanismen können durch dokumentiertes Review ihrer korrekten Verwendung belegt werden; eigene kritische Schutzlogik möglichst durch ausgeführte Negativtests.
- Pro Fall Status **belegt**, **offen**, **fehlgeschlagen** oder begründet **nicht anwendbar**; Belegart separat: automatisierter Test, manueller Test oder Review. Nicht vorhandene und nicht untersuchte Nachweise unterscheiden.
- Fortschritt = belegte Fälle / festgelegte anwendbare Fälle. Zähler/Nenner zeigen, Planversion und geprüften Commit angeben. Nicht aus zehn alten Bereichen einfach vier neue Pauschalpunkte ableiten.
- Automatisierungsgrad separat: erfolgreich automatisiert geprüfte Fälle / anwendbare Fälle. Review bleibt Review und wird nicht als Testlauf bezeichnet.
- Bekannte ausnutzbare Lücke bleibt Freigabeblocker unabhängig vom Gesamtwert. Ein bestandener Scanner oder zusätzliche Lint-Fälle können sie nicht kompensieren.
- Releasecheck separat: geprüfter Commit, zugeordnetes Paket, grüne vereinbarte Checks, bewertete Befunde und verantwortliche Person. ZIP-Installation einmal an der Paketgrenze nachweisen; kein doppelter Punkteabzug für denselben fehlenden Beleg.

Die Mehrleistung gegenüber typischen CI-Vorlagen ist damit **gezielte negative Prüfung eigener Schutzgrenzen und nachvollziehbare Fallzuordnung**, statt möglichst vieler Werkzeuge oder einer riesigen Matrix.

## Aktuelle Marketplace-Grenze

Die [offizielle Plugins FAQ](https://docs.moodle.org/503/en/Plugins_FAQ) beschreibt Einreichung, Archivvalidierung, Plugin CI und Review. Die [alte Contribution-Checklist](https://moodledev.io/general/community/plugincontribution/checklist) erklärt sich ausdrücklich für neue Einreichungen als überholt. Die verlinkten [aktuellen Submission Guidelines](https://moodle.atlassian.net/wiki/external/ODRhYjAyNTY4NDVmNGJlNjljN2ViMzkwYzdmYmIwMGI) lieferten im Webzugriff keinen lesbaren Inhalt. Deshalb keine abschließende Behauptung aktueller Marketplace-Testpflichten. Details und Guardians-Pause: [bestehende Moodle-Recherche](moodle-anforderungen.md).
