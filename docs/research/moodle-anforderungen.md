# Moodle-Anforderungen und Sicherheitschecks

Abrufdatum: **05.10.2026**. Grundlage: ausschließlich offizielle Moodle-Dokumentation und MoodleHQ-Projektquellen. Dies ist eine Bestandsaufnahme, keine Zertifizierung oder Aussage zur Rechtskonformität.

## Marketplace und ausgesetzte Reviews

- Moodle Marketplace hat das Plugins Directory ersetzt. Der alte Contribution-Prozess und seine Checklist sind ausdrücklich als **Legacy** gekennzeichnet und sollen für neue Einreichungen nicht verwendet werden. [Contribution](https://moodledev.io/general/community/plugincontribution), [Checklist](https://moodledev.io/general/community/plugincontribution/checklist).
- Die offizielle FAQ beschreibt weiterhin Einreichungen: ZIP mit eindeutigem Frankenstyle-Namen, Beschreibung, Dokumentation, Installationsanleitung und Screenshots; anschließend Archivvalidierung und Moodle-Plugin-CI-Checks, Review über ein Jira-Service-Management-Ticket und gegebenenfalls Nachbesserung. Freie Plugins werden einmalig geprüft, ohne garantierte Bearbeitungszeit; kostenpflichtige bei Einreichung und danach jährlich, typischerweise in zwei bis vier Wochen. Nach bestandenem Review veröffentlicht der Anbieter selbst. [Plugins FAQ, Abschnitt „How do I submit my own plugin to Moodle Marketplace?“](https://docs.moodle.org/503/en/Plugins_FAQ).
- **Nachweisbar pausiert ist das freiwillige Plugins-guardians-Programm** mit seinen bisherigen Peer-Reviews und QA-Prechecks. Moodle kündigt dessen Rückkehr für Marketplace an, ohne Termin. Daraus folgt keine belegte Aussetzung sämtlicher Marketplace-Einreichungen oder Zulassungsprüfungen. [Guardians](https://moodledev.io/general/community/plugincontribution/guardians), [QA prechecks](https://moodledev.io/general/community/plugincontribution/guardians/qaprechecks).

## Sicherheitsanforderungen als fachliche Grundlage

Die allgemeinen Moodle-Sicherheitsrichtlinien verlangen unter anderem folgende Schutzmaßnahmen. Diese gelten als Entwicklungsgrundlage unabhängig vom Wechsel der Veröffentlichungsplattform. [Security guidelines](https://moodledev.io/general/development/policies/security).

| Bereich | Zu prüfende Eigenschaft |
| --- | --- |
| Anmeldung und Kontext | `require_login()` beziehungsweise `require_course_login()` mit passendem Kurs-/Modulkontext; Ausnahmen begründen |
| Berechtigungen | Fähigkeiten vor Datenanzeige und Aktionen prüfen; Gruppen-/Kontextgrenzen beachten |
| CSRF | Zustandsändernde Formularaktionen über POST und gültige Sesskey absichern |
| Eingaben | Moodle-Formulare beziehungsweise typisierte Parameter-APIs verwenden; externe Daten ebenfalls als unvertrauenswürdig behandeln |
| XSS | Ausgaben passend zum Kontext maskieren beziehungsweise formatieren; privilegierte Ausnahmen kontrollieren |
| SQL-Injection | Moodle-Datenbank-API und gebundene Parameter verwenden |
| Shell-Injection | Shell-Aufrufe möglichst vermeiden; erforderliche Argumente korrekt maskieren |
| Nachvollziehbarkeit | Geeignete Moodle-Events und Tests mit problematischen Eingaben vorsehen |

Die **frühere** Contribution-Checklist nennt zusätzlich Datensparsamkeit, Offenlegung verarbeiteter personenbezogener Daten und Privacy-API-Implementierung bei externer Integration; für Aktivitäten Backup/Restore sowie PostgreSQL-Kompatibilität als Review-Themen. Diese Punkte bleiben sinnvolle interne Prüfkriterien; ihre genaue heutige Verbindlichkeit im Marketplace muss anhand der aktuellen Submission Guidelines bestätigt werden. [Legacy-Checklist, Privacy und Approval blockers](https://moodledev.io/general/community/plugincontribution/checklist).

## Was automatisierte Moodle-Checks nachweisen

| Prüfebene | Nachweis und Grenze | Quelle |
| --- | --- | --- |
| Directory-Code-Prechecks | PHP-Syntax, Coding Style für PHP/JS/CSS, PHPDoc, Upgrade-Savepoints, `thirdpartylibs.xml`, Grunt und Mustache; laut Dokumentation üblicherweise keine Prüfung der tatsächlichen Sicherheit, Funktionalität oder Codekorrektheit | [Code prechecks](https://moodledev.io/general/community/plugincontribution/codeprechecks) |
| `moodle-plugin-ci` | Werkzeug zum Ausführen von PHPUnit, Behat, Moodle Code Checker, PHPDoc, Mustache, Grunt, PHP-Lint und PHP Mess Detector; unterstützt Versions-/Datenbankmatrizen. Tatsächlich abgedeckt sind nur konfigurierte Befehle und implementierte Tests | [Moodle Plugin CI](https://moodlehq.github.io/moodle-plugin-ci/) |
| Manuelle frühere QA | Installation, Regression und Funktion; empfohlen: Developer Debugging, abweichendes Tabellenpräfix und PostgreSQL. Das zugehörige Guardians-Programm ist pausiert | [QA prechecks](https://moodledev.io/general/community/plugincontribution/guardians/qaprechecks) |

**Analyse:** Ein grüner Moodle-CI-Lauf allein belegt keine vollständige Abdeckung von Berechtigungen, Kursgrenzen, CSRF, XSS, Datenabfluss oder externen API-Zielen. Dafür braucht die Vereinsmatrix konkrete Sicherheitsfälle und deren Ergebnisse. Werkzeug vorhanden, Workflow konfiguriert, letzter erfolgreicher Lauf und nachgewiesene Sicherheitsfälle sind getrennte Angaben. Diese Schlussfolgerung ergibt sich aus den dokumentierten Grenzen der Prechecks und der testabhängigen Arbeitsweise von Plugin CI.

## Offene Nachweise

- Die aktuellen [Plugin submission guidelines](https://moodle.atlassian.net/wiki/spaces/MMPD/pages/3498180610/Plugin%2Bsubmission%2Bguidelines) und der [Marketplace Quick Guide](https://moodle.atlassian.net/wiki/spaces/MMPD/pages/3976560653/Quick%2Bguide%2Bto%2Blisting%2Ba%2Bplugin%2Bon%2BMoodle%2BMarketplace) waren im verwendeten Webzugriff durch eine Atlassian-Anmeldung nicht lesbar. Deshalb hier keine vermeintlich vollständige Liste aktueller Marketplace-Pflichtchecks.
- Die FAQ wurde zuletzt am 13.08.2026 bearbeitet; sie belegt den veröffentlichten Prozess, nicht die heutige tatsächliche Bearbeitungsgeschwindigkeit einzelner Tickets. Zusätzliche angekündigte Standards und konkrete Freigaben bleiben vor einer Veröffentlichung nachzuprüfen.
- Für eine Aussage, alle Reviews seien aktuell ausgesetzt, wurde keine offizielle Bestätigung gefunden. Die dokumentierte Guardians-Pause darf damit nicht gleichgesetzt werden.
- Aktuelle Marketplace-Regeln müssen vor Veröffentlichung erneut geprüft werden; alte Directory-Zulassung, Marketplace-Listing und eigene technische Sicherheitsfreigabe getrennt dokumentieren.
