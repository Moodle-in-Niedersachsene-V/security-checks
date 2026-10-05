# Vorschlag für einen gemeinsamen Prüfstandard

Stand: 05.10.2026. **Diskussionsentwurf**, noch keine beschlossene Vereinsrichtlinie oder bestätigte NLQ-Anforderung. Grundlage sind die belegten Projektchecks, [Moodle-Sicherheitsrichtlinien](https://moodledev.io/general/development/policies/security) und deren dokumentierte Grenzen. Für externe Webanwendungen bietet [OWASP ASVS](https://owasp.org/projects/asvs) eine ergänzende Prüfreferenz; ein konkretes ASVS-Level wurde hier nicht vereinbart.

## Einheitliche Nachweise, passende Werkzeuge

Ein Moodle-Plug-in läuft innerhalb Moodles. Ein externes Tool hat zusätzlich eigene Anmeldung, Server, Browser-Origin, Netzwerkschnittstellen und Tokenablage. Kurspilot enthält beide Teile; für eine Freigabe gehören beide in denselben Nachweis. Der gemeinsame Standard beschreibt geprüfte Schutzwirkungen, die konkrete Werkzeugwahl folgt dem Stack.

| ID | Bereich | Moodle-Plug-in | Externes Tool | Erwarteter Nachweis |
| --- | --- | --- | --- | --- |
| C01 | Qualität und Installation | PHP-Lint, Moodle Code Checker/PHPDoc, Struktur, Savepoints; Mustache/Grunt soweit vorhanden | Syntax-/Build-/Lint-Prüfungen für Node/Python und produktionsnaher Start | Erfolgreicher Lauf am Release-Commit; Installation des tatsächlichen Artefakts |
| C02 | Authentifizierung und Autorisierung | Login, Kontext, Capability, Kurs-/Gruppen-/Nutzergrenzen | Serverseitige Identität, Token-/Session-Gültigkeit, Rollen-/Mandantengrenzen | Negative Tests ohne Anmeldung, mit unberechtigter Rolle und fremder Objekt-ID |
| C03 | Zustandsänderungen | Formular-/AJAX-/Webservice-Schutz passend zur API, Sesskey und HTTP-Methode | CSRF bei Cookie-Authentifizierung; CORS/Origin und Token-Modell passend zur Architektur | Ungültige/fehlende Sesskey beziehungsweise Token ablehnen; Origin ist kein Identitätsnachweis |
| C04 | Eingaben und Ausgaben | Typisierte Parameter, gebundenes SQL, kontextgerechtes Escaping | Schema-/Parameterprüfung, gebundenes SQL, sichere HTML-/Markdown-Ausgabe | SQL-/XSS-/Pfad-Testfälle, manipulierte Parameter und externe Antworten |
| C05 | Dateien und Netzwerk | File API/Pluginfile, Eigentümer und Kontext prüfen; sichere externe Requests | Upload-/Download-Grenzen, Zielvalidierung, sichere TLS-Verbindungen | Fremde Dateien, Pfadtraversal, interne/verbotene Ziele, Redirects und Größenlimits testen |
| C06 | Abhängigkeiten und Geheimnisse | Composer/npm sowie eingebundene Drittbibliotheken berücksichtigen | npm/pip und Betriebskomponenten berücksichtigen | Passender Abhängigkeits-Audit und Secret-Scan; dokumentierte Bewertung jedes relevanten Befunds |
| C07 | Statische Sicherheitsanalyse | Moodle-Semgrep mit getesteten Regeln | Stackgerechte PHP-/JS-/Python-Regeln | Scanner-Lauf plus Fixtures; Ausnahmen mit Begründung und Verantwortlichem |
| C08 | Daten und Datenschutz | Privacy API soweit erforderlich, Export/Löschung, externe Datenflüsse und Events | Datensparsamkeit, Zugriff auf gespeicherte Daten, Löschung, Token-/Log-Redaktion | Tests für Zugriff und Löschung; dokumentierte Empfänger, Datenarten und Aufbewahrung |
| C09 | Kompatibilität und Upgrade | Unterstützte Moodle-/PHP-/DB-Stände aus realer Versionserklärung ableiten | Unterstützte Runtime-/OS-/Bereitstellungsstände | Mindestversion, eingesetzte Version und relevanter Upgradepfad getestet |
| C10 | Build-Kette und Freigabe | Wie beim externen Tool | Minimale CI-Rechte, kontrollierte Actions und reproduzierbares Artefakt | Release-SHA/Artefakt, erforderliche grüne Checks, bewertete Restbefunde, zuständige Person |

C02–C05 verlangen konkrete Angriffsfälle. Ein Scanner ersetzt diese Tests nicht. PHPUnit/Behat ohne eigene Testfälle erfüllt diese Prüfpunkte nicht. Hohe Liniencoverage ist hilfreich, sagt aber allein nichts über die getesteten Sicherheitsgrenzen aus.

Für Netzwerk-, Datei-, Admin- und KI-Funktionen sind die jeweiligen Zeilen besonders relevant. Bei KI-Tools zusätzlich prüfen, ob fremde Kurs-/Personendaten in Prompts oder Antworten gelangen und ob Modellantworten neue Aktionen ohne erneute Berechtigungsprüfung auslösen können. Diese Zusatzfälle sind aus den vorhandenen Integrationen abgeleitete Empfehlungen.

## Vorgehen bei Freigaben

1. Projektteile, Release-Commit und Artefakt festlegen. Veraltete Mirrors getrennt erfassen.
2. Für C01–C10 angeben: **bestanden**, **offen**, **fehlgeschlagen** oder **nicht anwendbar**. Nicht anwendbar begründen.
3. Befunde fachlich bewerten. Bestätigte ausnutzbare Sicherheitslücken vor einer regulären Freigabe beheben; Fehlalarme und akzeptierte Restrisiken ausdrücklich dokumentieren.
4. Vorgeschriebene Checks als Merge-/Release-Bedingungen einrichten. Ein Workflow-Eintrag allein beweist keine verbindliche Sperre.
5. Nachweis für die konkrete ausgelieferte Version archivieren; nach Änderungen erneut prüfen und veröffentlichte Abhängigkeiten weiter beobachten.

Bei allgemeinen Scannern sollte die Sperre von bewerteten Befunden abhängen. Bei Semgrep bedeutet die Schweregradbezeichnung einer Regel nicht automatisch dieselbe fachliche Risikoeinstufung. Der vorhandene videolearn-Aufruf nutzt `--error`; seine Wirkung und akzeptierte Ausnahmen gehören in die Freigabedokumentation.

GitHub dokumentiert [minimale Workflow-Rechte und das Fixieren von Actions auf vollständige Commit-SHAs](https://docs.github.com/en/actions/reference/security/secure-use). [GitHub Secret Scanning](https://docs.github.com/en/code-security/concepts/secret-security/secret-scanning) kann außerhalb eigener Workflow-Dateien laufen. Solche Plattformfunktionen und Branchregeln wurden in dieser Inventur nicht vollständig geprüft.

## Schlanker Nachweis pro Release

```markdown
Projekt und Komponenten:
Verantwortliche Person / Prüfdatum:
Release / Quell-Commit / Artefakt-ID oder SHA-256:
Unterstützte und getestete Moodle-/PHP-/DB-/Runtime-Stände:

| Prüfpunkt | Status | Testfall / Lauf / Bericht | Befund oder Begründung |
| --- | --- | --- | --- |
| C01–C10 jeweils einzeln | | | |

Offene Befunde mit Schweregrad, Zuständigkeit und Termin:
Ausnahmen und akzeptierte Restrisiken:
Freigabeentscheidung und freigebende Person:
```

Automatische Checks, manuelle Sicherheitsprüfung und eine institutionelle Freigabe sind unterschiedliche Nachweise. Die technischen Ergebnisse können eine Entscheidung des NLQ unterstützen; dessen konkrete Freigabekriterien müssen als eigene Quelle ergänzt werden.
