# Fortschritt im geplanten Prüfstandard

Bewertungsstand: **05.10.2026**, auf Basis der vorhandenen Bestandsaufnahme. Die Werte sind eine **vorläufige Bewertung der nachgewiesenen Abdeckung unseres Prüfplans**, keine gemessene Code-Coverage und keine Wahrscheinlichkeit, dass ein Projekt sicher ist. Neue Projektstände wurden für diese Darstellung nicht erneut untersucht.

## Skala

Der [Prüfstandard-Entwurf](pruefstandard-entwurf.md) hat zehn Bereiche C01–C10. Jeder Bereich zählt gleich viel:

| Punkte je Bereich | Bewertungsregel |
| --- | --- |
| 10 | Wesentliche Schutzwirkung durch passende, gelesene Prüfungen und erfolgreiche Ausführung am geprüften Stand belegt |
| 5 | Teilabdeckung: passende Prüfungen vorhanden, aber Umfang, Ausführung oder weitere Nachweise fehlen |
| 0 | In der bisherigen Erhebung kein geeigneter Nachweis gefunden |

**Prozentwert = Summe der Punkte / 100 × 100.** Die Balken haben 20 Felder à 5 Prozentpunkte. Ein einzelner Bereich erreicht 10 Punkte für die belegte wesentliche Schutzwirkung, nicht als Behauptung, dass jede denkbare Angriffskombination getestet wurde.

0 Punkte bedeuten **nicht nachgewiesen**; unbekannte lokale oder interne Tests erhalten keine erfundenen Punkte. Bekannte Lücken verhindern volle Punkte im betroffenen Bereich. Bloße PHPUnit-/Behat-Aufrufe ohne eigene Tests erhalten für C02–C05 keine Punkte. Eine Privacy-Implementierung allein zählt nicht als erfolgreich getesteter Datenschutz.

Alle zehn Bereiche bleiben vorläufig im Nenner. Eine spätere Einstufung als „nicht anwendbar“ muss begründet werden; anschließend lautet die Berechnung Punkte / (10 × anwendbare Bereiche). Die Gleichgewichtung zeigt den Umsetzungsfortschritt. Kritische Einzelbefunde und deren Priorität bleiben unabhängig vom Gesamtwert bestehen.

## Übersicht je Projekt

| Projekt | Prüfplan-Abdeckung | Fortschritt | Einordnung |
| --- | ---: | --- | --- |
| Kurspilot – native Linie | **50 %** | ██████████░░░░░░░░░░ | Rechte-/OAuth-/Datei-Negativtests und erfolgreiche CI; Scanner, weitere Plattformen und vollständige Freigabenachweise offen. |
| MooGPT | **15 %** | ███░░░░░░░░░░░░░░░░░ | Lokale Suite und einzelne Auth-/Origin-Tests vorhanden; kein Laufnachweis, Identitäts-/Origin-Lücken und nachgebauter XSS-Test. |
| Kurspilot – älterer Mirror | **5 %** | █░░░░░░░░░░░░░░░░░░░ | Paket-/Metadatenprüfung im Export; keine eigenen Tests oder CI des Mirror-Stands nachgewiesen. |
| mod_videolearn | **20 %** | ████░░░░░░░░░░░░░░░░ | Zusätzlich Semgrep am geprüften HEAD erfolgreich; Regelfixtures und negative Sicherheitstests fehlen. |
| local_nextcloudbackup | **15 %** | ███░░░░░░░░░░░░░░░░░ | Moodle-CI und Versionsmatrix vorhanden; erfolgreicher Lauf am geprüften HEAD, keine eigenen Testdateien. |
| block_kursfilter | **15 %** | ███░░░░░░░░░░░░░░░░░ | Moodle-CI und Versionsmatrix vorhanden; erfolgreicher Lauf am geprüften HEAD, keine eigenen Testdateien. |
| local_easyadmin | **15 %** | ███░░░░░░░░░░░░░░░░░ | Moodle-CI und Versionsmatrix vorhanden; erfolgreicher Lauf am geprüften HEAD, keine eigenen Testdateien. |
| local_calendarcategories | **15 %** | ███░░░░░░░░░░░░░░░░░ | Moodle-CI und Versionsmatrix vorhanden; erfolgreicher Lauf am geprüften HEAD, keine eigenen Testdateien. |
| local_peertubeupload | **15 %** | ███░░░░░░░░░░░░░░░░░ | Moodle-CI und Versionsmatrix vorhanden; erfolgreicher Lauf am geprüften HEAD, keine eigenen Testdateien. |
| tiny_textgestaltung | **15 %** | ███░░░░░░░░░░░░░░░░░ | Moodle-CI und Versionsmatrix vorhanden; erfolgreicher Lauf am geprüften HEAD, keine eigenen Testdateien. |
| repository_peertubeoauth | **15 %** | ███░░░░░░░░░░░░░░░░░ | Moodle-CI und Versionsmatrix vorhanden; erfolgreicher Lauf am geprüften HEAD, keine eigenen Testdateien. |
| local_emailclient | **15 %** | ███░░░░░░░░░░░░░░░░░ | Moodle-CI und Versionsmatrix vorhanden; erfolgreicher Lauf am geprüften HEAD, keine eigenen Testdateien. |
| repository_nextclouddirect | **15 %** | ███░░░░░░░░░░░░░░░░░ | Moodle-CI und Versionsmatrix vorhanden; erfolgreicher Lauf am geprüften HEAD, keine eigenen Testdateien. |
| block_questionfilter | **10 %** | ██░░░░░░░░░░░░░░░░░░ | CI und Versionsmatrix vorhanden; jüngster gelesener Lauf nicht am geprüften HEAD, keine eigenen Testdateien. |
| local_kursassistent | **10 %** | ██░░░░░░░░░░░░░░░░░░ | CI und Versionsmatrix vorhanden; jüngster gelesener Lauf nicht am geprüften HEAD, keine eigenen Testdateien. |
| local_plugininfo | **0 %** | ░░░░░░░░░░░░░░░░░░░░ | In der bisherigen Inventur keine automatisierten Prüfungen oder eigenen Testdateien nachgewiesen. |
| block_mrbs_nds | **0 %** | ░░░░░░░░░░░░░░░░░░░░ | In der bisherigen Inventur keine automatisierten Prüfungen oder eigenen Testdateien nachgewiesen. |
| moodle_dashboard | **0 %** | ░░░░░░░░░░░░░░░░░░░░ | In der bisherigen Inventur keine automatisierten Prüfungen oder eigenen Testdateien nachgewiesen. |

Quellen und geprüfte Commits: [Vereinsprojekte](research/verein-checks.md), [Vereinsinventur](research/verein-inventur.json) und [eigene Projekte](research/eigene-projekte.md). Die Zahlen werden aus diesen Berichten abgeleitet; sie sind kein zusätzlicher Testlauf. grAIde bleibt ausgeschlossen. Als bayerische Referenz wurde ergänzend der AI Chat anhand desselben Schemas bewertet.

### Bayerische Referenz: AI Chat

| Projekt | Prüfplan-Abdeckung | Fortschritt | Einordnung |
| --- | ---: | --- | --- |
| ByCS AI Chat (`block_ai_chat`) | **35 %** | ███████░░░░░░░░░░░░░ | Erfolgreiche CI, negative Persona-/Capability-Tests, Ausgabe-Bereinigung und private Wiederherstellung; weitere Sicherheitsbereiche nicht nachgewiesen. |

Bewertung für Commit [`a9aa7be9`](https://github.com/bycs-lp/moodle-block_ai_chat/commit/a9aa7be9f3b2aa164891c08f096043441018895e), ergänzend geprüft am 05.10.2026. Der [CI-Lauf vom 28.09.2026](https://github.com/bycs-lp/moodle-block_ai_chat/actions/runs/36413841952) ist an diesem Stand erfolgreich. Er verwendet die gemeinsame ByCS-Vorlage auf Commit `dbc54d01`. 16 Testjobs decken Moodle 5.0/5.1/5.2 und `main`, PHP 8.3/8.4 sowie PostgreSQL/MariaDB ab.

Im näher gelesenen [Moodle-5.0/PostgreSQL-Job](https://github.com/bycs-lp/moodle-block_ai_chat/actions/runs/36413841952/job/108900201155) liefen **18 PHPUnit-Tests mit 118 Assertions** erfolgreich; das Log nennt außerdem 16 PHPUnit-Deprecations. Keine gemessene Liniencoverage. Der grüne Behat-Schritt zählt nicht als eigene Browser-Testabdeckung: Im geprüften AI-Chat-Dateibaum fehlen eigene `.feature`-Dateien.

| Bereich | Punkte | Nachweis / Grenze |
| --- | ---: | --- |
| C01 Qualität / Installation | 5 | Erfolgreiche Installation, PHP-Lint, PHPCS, Validierung, Savepoints, Mustache und Grunt; kein Nachweis der Installation des tatsächlich veröffentlichten ZIPs |
| C02 Authentifizierung / Rechte | 10 | Produktive Manager-Funktionen verweigern fremde Personas und fehlende Capabilities; Rechte beim Bearbeiten und Eigentümererhalt werden getestet. Kein Vollnachweis aller Endpunkte |
| C03 Zustandsänderungen | 0 | Keine negativen Sesskey-/CSRF-/HTTP-Methodenfälle in den gelesenen eigenen Tests nachgewiesen; Capability-Tests werden bereits in C02 gezählt |
| C04 Eingaben / Ausgaben | 5 | `test_edit_persona` und `test_get_personas` prüfen Entfernung von Script-Tags aus Ausgaben; kein vollständiger SQL-/XSS-/Browser-Testkatalog |
| C05 Dateien / Netzwerk | 0 | Keine geeigneten negativen Datei-/SSRF-/TLS-/Netzwerkfälle im AI Chat nachgewiesen; funktionierende Backups allein erfüllen diesen Bereich nicht |
| C06 Abhängigkeiten / Secrets | 0 | Keine geeigneten Audit-/Secret-Scan-Nachweise in der bisherigen Erhebung |
| C07 Statische Sicherheitsanalyse | 0 | Keine dedizierte Sicherheits-SAST in den untersuchten Workflows; PHPCS wird unter C01 gezählt |
| C08 Daten / Datenschutz | 5 | Restore erzeugt private Personas im Eigentum der wiederherstellenden Person; Blocklöschung räumt eigene Daten auf und lässt andere Blöcke bestehen. Vollständige Privacy-Export-/Nutzerlösch-/Aufbewahrungsprüfung nicht belegt |
| C09 Kompatibilität / Upgrade | 5 | Breite erfolgreiche Versions-/DB-Matrix, Backup/Restore und Test einer Upgrade-Cleanup-Funktion; vollständiger Versionsupgrade des veröffentlichten Artefakts nicht belegt |
| C10 Build-Kette / Freigabe | 5 | Erfolgreicher Lauf am fixierten Commit; Release-Workflow vorhanden. Verbindlicher Branchschutz, geprüfter Release-ZIP und vollständige Freigabedokumentation nicht verifiziert |
| **Gesamt** | **35 / 100** | **35 % nach derselben vorläufigen Skala** |

Direkte Testquellen: [Manager und Persona-Rechte/Ausgaben](https://github.com/bycs-lp/moodle-block_ai_chat/blob/a9aa7be9f3b2aa164891c08f096043441018895e/tests/manager_test.php), [private Persona-Wiederherstellung](https://github.com/bycs-lp/moodle-block_ai_chat/blob/a9aa7be9f3b2aa164891c08f096043441018895e/tests/backup_restore_test.php), [Blocklöschung](https://github.com/bycs-lp/moodle-block_ai_chat/blob/a9aa7be9f3b2aa164891c08f096043441018895e/tests/block_ai_chat_test.php), [Upgrade-Cleanup](https://github.com/bycs-lp/moodle-block_ai_chat/blob/a9aa7be9f3b2aa164891c08f096043441018895e/tests/upgradelib_test.php), [Release-Workflow](https://github.com/bycs-lp/moodle-block_ai_chat/blob/a9aa7be9f3b2aa164891c08f096043441018895e/.github/workflows/moodle-release.yml). Ergänzender Überblick: [ByCS-Inventur](research/bayern-checks.md).

Der Wert gilt für den **AI-Chat-Baustein**, nicht für die gesamte KI-Suite oder den bayerischen Betrieb. Tests des separaten AI Managers und interne Audits werden nicht automatisch übertragen. Eine Prüfung des vollständigen Installationspakets muss die Abhängigkeiten ausdrücklich einbeziehen. Die hier erneut gelesenen Testdateien und vorhandenen GitHub-Läufe wurden nicht durch neue Testläufe ergänzt.

## Nachvollziehbare Punktevergabe

| Projekt | C01 | C02 | C03 | C04 | C05 | C06 | C07 | C08 | C09 | C10 | Gesamt |
| --- | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: |
| Kurspilot – native Linie | 5 | 10 | 5 | 5 | 10 | 0 | 0 | 5 | 5 | 5 | **50 %** |
| ByCS AI Chat – Referenz | 5 | 10 | 0 | 5 | 0 | 0 | 0 | 5 | 5 | 5 | **35 %** |
| MooGPT | 5 | 5 | 5 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | **15 %** |
| Kurspilot – älterer Mirror | 5 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | **5 %** |
| mod_videolearn | 5 | 0 | 0 | 0 | 0 | 0 | 5 | 0 | 5 | 5 | **20 %** |
| local_nextcloudbackup | 5 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 5 | 5 | **15 %** |
| block_kursfilter | 5 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 5 | 5 | **15 %** |
| local_easyadmin | 5 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 5 | 5 | **15 %** |
| local_calendarcategories | 5 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 5 | 5 | **15 %** |
| local_peertubeupload | 5 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 5 | 5 | **15 %** |
| tiny_textgestaltung | 5 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 5 | 5 | **15 %** |
| repository_peertubeoauth | 5 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 5 | 5 | **15 %** |
| local_emailclient | 5 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 5 | 5 | **15 %** |
| repository_nextclouddirect | 5 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 5 | 5 | **15 %** |
| block_questionfilter | 5 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 5 | 0 | **10 %** |
| local_kursassistent | 5 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 5 | 0 | **10 %** |
| local_plugininfo | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | **0 %** |
| block_mrbs_nds | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | **0 %** |
| moodle_dashboard | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | **0 %** |

Die Spalten entsprechen den zehn Bereichen des Prüfstandard-Entwurfs:

| Bereich | Kurzname | Grund für Teilpunkte in der aktuellen Bewertung |
| --- | --- | --- |
| C01 | Qualität / Installation | Vereins-CI: Qualität und Installation, aber kein Nachweis des ausgelieferten ZIPs; Kurspilot: ZIP-Prüfung, aber fehlende Moodle-Style-/Template-Checks; MooGPT: lokale Suite ohne Ausführungsnachweis; Mirror: nur Exportprüfungen |
| C02 | Authentifizierung / Rechte | Kurspilot: konkrete Kurs-/Nutzer-/Token-Negativtests mit erfolgreicher Suite → 10; MooGPT: einzelne echte Auth-Tests, aber Schüleridentität und Origin-Grenzen offen → 5 |
| C03 | Zustandsänderungen | Kurspilot: OAuth-/Token-Negativfälle, zusätzliche UI-/CSRF-Nachweise offen; MooGPT: einzelne Token-/Origin-Tests ohne Laufnachweis und mit bekannten Lücken |
| C04 | Eingaben / Ausgaben | Kurspilot: Pfad- und ungültige Importdaten getestet; kompletter SQL-/XSS-Katalog nicht belegt. MooGPTs nachgebauter Renderer-Test zählt nicht als Produktionsnachweis |
| C05 | Dateien / Netzwerk | Kurspilot: produktive Pfad-/Downloadticket-/WebDAV-Komponenten mit negativen Assertions und erfolgreicher Suite → 10; Live-DNS-/Netzwerkumgehungen bleiben zusätzliche Prüffälle |
| C06 | Abhängigkeiten / Secrets | Keine geeigneten Audit-/Scan-Nachweise in der bisherigen Erhebung |
| C07 | Statische Sicherheitsanalyse | videolearn: erfolgreicher Semgrep-Lauf, aber Regelfixtures und dokumentierte Ausnahmen nicht vollständig belegt → 5 |
| C08 | Daten / Datenschutz | Kurspilot: technische Export-/Lösch-/Token-Tests; vollständige Datenfluss-/Aufbewahrungsnachweise nicht bewertet → 5 |
| C09 | Kompatibilität / Upgrade | Vereins-CI: Versionsmatrix, aber Mindeststände, einzelne DB-Versionen und Upgradefälle offen; Kurspilot: 5.0/5.1 mit MariaDB, zusätzliche Plattform-/Upgrade-Nachweise offen |
| C10 | Build-Kette / Freigabe | Vereins-CI: grüner Lauf am geprüften HEAD, aber Artefakt-/Gate-/Freigabedokumentation offen → 5; fehlt dieser HEAD-Lauf, 0. Kurspilot: SHA/ZIP und Gate-Workflow, Branchschutz und vollständige Freigabe nicht verifiziert → 5 |

## Code-Coverage separat

| Projekt | Gemessene Code-Coverage | Bezug |
| --- | ---: | --- |
| Kurspilot – natives PHP | **80,9 %** | Liniencoverage im gelesenen Moodle-5.0-CI-Job; nicht das ganze Produkt und nicht der Prüfplan |
| Alle übrigen bewerteten Projektstände | **Nicht erhoben / nicht nachgewiesen** | Aus Dateizahlen, Lint oder Scanner-Ergebnissen lässt sich keine Code-Coverage berechnen |

Quelle zum gemessenen Wert: [Kurspilot-CI-Job](https://github.com/matthiasgruenwald/moodle-coursepilot/actions/runs/36885192594/job/110446882744), erläutert im [Projektbericht](research/eigene-projekte.md).

## Wie die Werte steigen

- **Vereins-Plug-ins:** Eigene negative Rechte-, Sesskey-, Eingabe- und Datei-/Netzwerktests ergänzen und am Release-Stand ausführen. Vier vollständig belegte zusätzliche Bereiche ergeben 40 weitere Prozentpunkte.
- **MooGPT:** Identitäts-/Origin-Lücken schließen und Tests der echten Produktfunktionen in CI ausführen. Vorhandene lokale Tests können dann zu belegter Teil- oder Vollabdeckung werden.
- **Kurspilot:** Die bereits belegten Sicherheitsfälle um Eingabe-/Ausgabe-, UI-/CSRF-, Plattform- und Datenflussnachweise ergänzen; Abhängigkeits-/Secret-Scans und SAST schließen weitere Bereiche.
- **Mirror:** Den tatsächlich veröffentlichten Plugin-Stand prüfen. Die 50 % der nativen Linie lassen sich nicht auf ein anderes Artefakt übertragen.

Bei jedem neuen Nachweis die betreffende Zelle, die Gesamtsumme und den Quellenstand aktualisieren. Ein Wert von 90 % wäre erst mit 90 belegten Punkten gerechtfertigt.
