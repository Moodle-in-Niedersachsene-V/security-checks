# Fortschritt und Testabdeckung

Stand: **05.10.2026, überarbeitet nach Moodle-Recherche**. Die frühere C01–C10-Punkteskala einschließlich **35 % für AI Chat** wird zurückgezogen. Gleichgewichtete Werkzeuge, doppelte Freigabenachweise und pauschal anwendbare Bereiche waren keine belastbare Grundlage. Die alten Zahlen sind im [Commit 8d762c0](https://github.com/Moodle-in-Niedersachsene-V/security-checks/blob/8d762c0/docs/pruefplan-abdeckung.md) erhalten; sie werden nicht als aktuelle Projektwerte fortgeführt.

Der [neue Entwurf](pruefstandard-entwurf.md) zählt konkrete anwendbare Prüffälle. Die [Kalibrierungsrecherche](research/moodle-pruefstandard-kalibrierung.md) begründet das anhand offizieller Moodle-Dokumentation sowie Attendance, Checklist und ObjectFS.

## Wie Prozentwerte künftig entstehen

| Kennzahl | Berechnung | Aussage |
| --- | --- | --- |
| Prüfplan-Fortschritt | Belegte Fälle / alle festgelegten anwendbaren Fälle × 100 | Wie weit der vereinbarte Plan durch Review oder ausgeführte Tests belegt ist |
| Automatisierte Testabdeckung des Plans | Erfolgreich automatisiert geprüfte Fälle / anwendbare Fälle × 100 | Wie viel des Plans automatisiert geprüft wird |
| Code-Coverage | Gemessene erreichte Codezeilen / berücksichtigte Codezeilen | Wie viel Code in der jeweiligen Suite ausgeführt wurde |

Zähler/Nenner, Planversion und Commit gehören zum Wert. Ein Review wird nicht als ausgeführter Test gezählt. Offene und fehlgeschlagene Fälle bleiben im Nenner; nicht anwendbare Fälle brauchen eine Begründung. Fälle sinnvoll nach Schutzgrenzen zuschneiden, nicht mit vielen Lint-Einzelfällen den Wert erhöhen. Nach Planänderungen den Umfang mit ausweisen.

**Rechenbeispiel, kein Projektwert:** 12 vereinbarte anwendbare Fälle, davon 9 belegt und 6 erfolgreich automatisiert geprüft → 75 % Prüfplan-Fortschritt und 50 % automatisierte Testabdeckung. 100 % bedeutet vollständigen Nachweis dieses Plans. Eine bekannte ausnutzbare Lücke bleibt unabhängig davon ein Freigabeblocker.

Für die erfassten Projekte fehlt noch die vollständige Fallzuordnung nach diesem engeren Plan. Deshalb aktuell **kein neu berechneter Gesamtprozentwert**. Das bedeutet weder 0 % noch eine Sicherheitsabwertung. Die Tabelle hält die bereits erhobenen Nachweise fest; neue Tests wurden dafür nicht ausgeführt.

## Übersicht je Projekt

| Projekt | Bereits belegte beziehungsweise gefundene Prüfungen | Nächster sinnvoller Nachweis |
| --- | --- | --- |
| Kurspilot – native Linie | Erfolgreiche CI; echte negative OAuth-, Rechte-, Datei-/Netzwerk- und Privacy-Fälle | Fallplan zuordnen; eigene Eingabe-/Ausgabe- und UI-Schutzgrenzen ergänzend prüfen; zugesagte Plattformen berücksichtigen |
| MooGPT | Lokale Suite mit einzelnen echten Auth-/Origin-Tests; kein Laufnachweis am untersuchten Stand | Identitäts-/Origin-Befunde klären und absichern; Tests produktiver Funktionen in CI ausführen |
| Kurspilot – älterer Mirror | Export-/Metadatenprüfungen; keine eigene CI/Testausführung nachgewiesen | Tatsächlich verteilten Stand und dessen Funktion separat prüfen |
| mod_videolearn | Moodle-CI und Semgrep am geprüften HEAD erfolgreich; keine eigenen Testdateien gefunden | Eigene Rechte-/Schreib-/Ausgabefälle; Semgrep-Regeln vor Übernahme verifizieren |
| local_nextcloudbackup | Moodle-CI am geprüften HEAD erfolgreich; keine eigenen Testdateien gefunden | Basisfälle und relevante Backup-/Datei-/Netzwerk-/Datenfälle festlegen |
| block_kursfilter | Moodle-CI am geprüften HEAD erfolgreich; keine eigenen Testdateien gefunden | Basisfälle; relevante Kurs-/Rollen-/Sichtbarkeitsgrenzen prüfen |
| local_easyadmin | Moodle-CI am geprüften HEAD erfolgreich; keine eigenen Testdateien gefunden | Basisfälle; eigene administrative Rechte und Schreibaktionen prüfen |
| local_calendarcategories | Moodle-CI am geprüften HEAD erfolgreich; keine eigenen Testdateien gefunden | Basisfälle; eigene Sichtbarkeits- und Schreibgrenzen prüfen |
| local_peertubeupload | Moodle-CI am geprüften HEAD erfolgreich; keine eigenen Testdateien gefunden | Basisfälle und vorhandene Upload-/Netzwerk-/Tokenpfade prüfen |
| tiny_textgestaltung | Moodle-CI am geprüften HEAD erfolgreich; keine eigenen Testdateien gefunden | Basisfälle; tatsächlich verarbeitete Texte und Ausgabe prüfen |
| repository_peertubeoauth | Moodle-CI am geprüften HEAD erfolgreich; keine eigenen Testdateien gefunden | Basisfälle und eigene OAuth-/Datei-/Netzwerkgrenzen prüfen |
| local_emailclient | Moodle-CI am geprüften HEAD erfolgreich; keine eigenen Testdateien gefunden | Basisfälle; relevante Nachrichten-/Empfänger-/Daten-/Netzwerkgrenzen prüfen |
| repository_nextclouddirect | Moodle-CI am geprüften HEAD erfolgreich; keine eigenen Testdateien gefunden | Basisfälle und vorhandene Datei-/Netzwerk-/Tokenpfade prüfen |
| block_questionfilter | CI konfiguriert; gelesener letzter Lauf nicht am geprüften HEAD; keine eigenen Testdateien gefunden | Aktuellen Lauf und eigene Zugriffs-/Ausgabefälle belegen |
| local_kursassistent | CI konfiguriert; gelesener letzter Lauf nicht am geprüften HEAD; keine eigenen Testdateien gefunden | Aktuellen Lauf und eigene Rechte-/Schreib-/Ausgabefälle belegen |
| local_plugininfo | Keine automatisierten Prüfungen/eigenen Testdateien in der Inventur gefunden | Schlanke Qualitäts-CI und anwendbare Basisfälle festlegen |
| block_mrbs_nds | Keine automatisierten Prüfungen/eigenen Testdateien in der Inventur gefunden | Schlanke Qualitäts-CI und eigene Buchungs-/Rechte-/Datenfälle prüfen |
| moodle_dashboard | Externes Tool; keine automatisierten Prüfungen/eigenen Testdateien gefunden | Passenden Web-/Identitäts-/Token-/Daten-/Netzwerkplan festlegen |

Die Vorschläge für noch nicht vertieft untersuchte Projektfunktionen sind Ausgangspunkte zur Fallplanung, keine neu bewiesenen Implementierungslücken. Quellen und geprüfte Commits: [Vereinsprojekte](research/verein-checks.md), [Vereinsinventur](research/verein-inventur.json), [eigene Projekte](research/eigene-projekte.md). grAIde bleibt ausgeschlossen.

## Bayerische Referenz: AI Chat

**Die frühere Zahl 35 % wird nicht durch eine geschätzte 90-%-Zahl ersetzt.** Bereits nachgewiesen sind eine breite erfolgreiche Qualitäts-/Versions-CI und konkrete sicherheitsrelevante Tests. Fehlende separate Scanner rechtfertigen keine pauschale Abwertung. Für eine belastbare neue Prozentzahl müssten zuerst die eigenen Endpunkte und anwendbaren Prüffälle vollständig zugeordnet werden.

Geprüfter Stand: Commit [`a9aa7be9`](https://github.com/bycs-lp/moodle-block_ai_chat/commit/a9aa7be9f3b2aa164891c08f096043441018895e), ergänzend geprüft am 05.10.2026. Der [CI-Lauf vom 28.09.2026](https://github.com/bycs-lp/moodle-block_ai_chat/actions/runs/36413841952) ist an diesem Stand erfolgreich. Er verwendet die gemeinsame ByCS-Vorlage auf Commit `dbc54d01`. 16 Testjobs decken Moodle 5.0/5.1/5.2 und `main`, PHP 8.3/8.4 sowie PostgreSQL/MariaDB ab.

Im näher gelesenen [Moodle-5.0/PostgreSQL-Job](https://github.com/bycs-lp/moodle-block_ai_chat/actions/runs/36413841952/job/108900201155) liefen **18 PHPUnit-Tests mit 118 Assertions** erfolgreich; das Log nennt außerdem 16 PHPUnit-Deprecations. Keine gemessene Liniencoverage. Der grüne Behat-Schritt zählt nicht als eigene Browser-Testabdeckung: Im geprüften AI-Chat-Dateibaum fehlen eigene `.feature`-Dateien.

| Neuer Bereich / Modul | Bereits gelesener Nachweis | Noch zu klären |
| --- | --- | --- |
| B1 Qualität / Kompatibilität | Installation, PHP-Lint, PHPCS, Validate, Savepoints, Mustache/Grunt und eigene PHPUnit-Tests in erfolgreicher breiter Matrix | Zuordnung zur tatsächlich verteilten Version |
| B2 Berechtigungen | Fremde Personas und fehlende Capabilities abgewiesen; Bearbeitungsrechte und Eigentümererhalt getestet | Abdeckung weiterer unterschiedlicher Einstiegspunkte; kein pauschaler Vollnachweis |
| B3 Zustandsänderungen | Eigene Capability-Tests vorhanden | Form-/AJAX-/Webservice-Schutz im Code nachvollziehen; bei eigener Schutzlogik fehlende/ungültige Sesskey oder Tokens testen. Bisher kein entsprechender Negativfall belegt. |
| B4 Eingaben / Ausgaben | Bearbeiten und Abrufen von Personas prüft Entfernung von Script-Tags | Weitere eigene Ausgabewege einschließlich Modellantworten; vorhandene Moodle-Schutzmechanismen korrekt eingebunden? |
| Backup / Restore / Löschung | Restore erzeugt private Personas im Eigentum der wiederherstellenden Person; Blocklöschung bewahrt andere Blöcke; Upgrade-Cleanup getestet | Passende Privacy-Export-/Nutzerlöschfälle zuordnen; die vorhandenen Löschtests sind kein vollständiger Privacy-Nachweis |
| KI / Netzwerk / Abhängigkeiten | Abhängigkeit von AI Manager und Tiny AI bekannt | Welche Schutzgrenzen liegen im AI Chat, welche bei Abhängigkeiten? Nur tatsächlich eigene Netzwerk-/Tokenlogik dem AI Chat zuordnen. |

Direkte Testquellen: [Manager und Persona-Rechte/Ausgaben](https://github.com/bycs-lp/moodle-block_ai_chat/blob/a9aa7be9f3b2aa164891c08f096043441018895e/tests/manager_test.php), [private Persona-Wiederherstellung](https://github.com/bycs-lp/moodle-block_ai_chat/blob/a9aa7be9f3b2aa164891c08f096043441018895e/tests/backup_restore_test.php), [Blocklöschung](https://github.com/bycs-lp/moodle-block_ai_chat/blob/a9aa7be9f3b2aa164891c08f096043441018895e/tests/block_ai_chat_test.php), [Upgrade-Cleanup](https://github.com/bycs-lp/moodle-block_ai_chat/blob/a9aa7be9f3b2aa164891c08f096043441018895e/tests/upgradelib_test.php), [Release-Workflow](https://github.com/bycs-lp/moodle-block_ai_chat/blob/a9aa7be9f3b2aa164891c08f096043441018895e/.github/workflows/moodle-release.yml). Ergänzender Überblick: [ByCS-Inventur](research/bayern-checks.md).

Die Nachweise beziehen sich auf den **AI-Chat-Baustein**. Die gesamte KI-Suite und der bayerische Betrieb benötigen einen eigenen Prüfungsumfang. Tests des separaten AI Managers und interne Audits werden nicht automatisch übertragen. Eine Prüfung des vollständigen Installationspakets muss die Abhängigkeiten ausdrücklich einbeziehen. Die hier erneut gelesenen Testdateien und vorhandenen GitHub-Läufe wurden nicht durch neue Testläufe ergänzt.

## Code-Coverage separat

| Projekt | Gemessene Code-Coverage | Bezug |
| --- | ---: | --- |
| Kurspilot – natives PHP | **80,9 %** | Liniencoverage im gelesenen Moodle-5.0-CI-Job; nicht das ganze Produkt und nicht der Prüfplan |
| Alle übrigen erfassten Projektstände | **Nicht erhoben / nicht nachgewiesen** | Dateizahlen, Lint und Scanner erlauben keine Berechnung |

Quelle: [Kurspilot-CI-Job](https://github.com/matthiasgruenwald/moodle-coursepilot/actions/runs/36885192594/job/110446882744), erläutert im [Projektbericht](research/eigene-projekte.md). Der Wert bleibt als echte Messung erhalten; 80 % werden dadurch nicht zur universellen Vereinsanforderung.
