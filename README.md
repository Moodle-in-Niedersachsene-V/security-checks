# Sicherheitschecks

Dieses Repository sammelt und vergleicht die Sicherheitschecks der Projekte von Moodle in Niedersachsen e. V., MooGPT und Kurspilot. Ziel ist ein einheitlicher, nachvollziehbarer Prüfstandard als Grundlage für Wartung und die technische Bewertung durch das NLQ.

**Bestandsaufnahme: 05.10.2026.** Erfasst wurden 15 öffentliche Vereinsrepositories, die genannten eigenen Projekte einschließlich Kurspilot-Mirror und alle 101 öffentlich gefundenen ByCS-Repositories. grAIde bleibt auf Wunsch außerhalb des Vergleichs.

## Einstieg

| Dokument | Inhalt |
| --- | --- |
| [Fortschritt und Testabdeckung](docs/pruefplan-abdeckung.md) | Fallbezogene Prozentberechnung, belegter Stand je Projekt und AI Chat; alte Pauschalwerte zurückgezogen |
| [Vereinsprojekte](docs/research/verein-checks.md) | Tabelle je Projekt, konfigurierte Checks, jüngste sichtbare CI-Läufe und Lücken |
| [MooGPT und Kurspilot](docs/research/eigene-projekte.md) | Externe Tools, native Moodle-Komponente, ältere Mirror-Linie und konkrete Sicherheitsfälle |
| [Bayerische Referenzen](docs/research/bayern-checks.md) | Gesamttabelle aller öffentlichen ByCS-Projekte; vertiefter Blick auf KI-Manager und AI-Chat |
| [Moodle-Anforderungen](docs/research/moodle-anforderungen.md) | Aktueller Marketplace-Prozess, Guardians-Pause und Grenzen automatisierter Moodle-Checks |
| [Prüfstandard – Entwurf v2](docs/pruefstandard-entwurf.md) | Vier Basisbereiche, funktionsabhängige Module und schlanke Nachweise |
| [Vorschlag für Betrieb und Wartbarkeit beim NLQ](docs/nlq-betrieb-und-wartbarkeit.md) | Betreuung, Übernahme, Supportzeitraum und Entscheidung über Weiterbetrieb, Nachbesserung oder Ablösung |
| [Ergänzender Qualitätsablauf](docs/qualitaetsablauf-entwurf.md) | Ratsche, gemeinsame Gates, Coverage/CRAP und gezielte Mutation als Vorschläge zur wartbaren Entwicklung |
| [Easy-Admin-Pilot und Größenvergleich](docs/pilots/easyadmin/README.md) | Neun ausgeführte Sicherheitstests, zwei wirksame Gegenproben und übernehmbarer Plugin-Patch |
| [Pilotplanung für alle Vereinsplugins](docs/pilotplanung-verein.md) | Größen aller 14 Plugins, Funktionsvergleich und geplanter Prüfungsumfang für Kursfilter; keine neue Umsetzung |
| [Kalibrierung an Moodle und etablierten Plugins](docs/research/moodle-pruefstandard-kalibrierung.md) | Offizielle Richtlinien und konkrete Testpraxis bei Attendance, Checklist und ObjectFS |
| [Vereinsinventur](docs/research/verein-inventur.json) / [ByCS-Inventur](docs/research/bycs-inventory.json) | Maschinenlesbare Quellen, Commit-Stände und Prüfkonfigurationen |

Der erste Entwurf mit zehn gleich gewichteten Bereichen und Werten wie „35 % für AI Chat“ wurde nach der Moodle-Recherche zurückgezogen. Der überarbeitete Plan verlangt passende Schutzprüfungen und zusätzliche Werkzeuge nach Risiko. Neue Gesamtwerte werden erst anhand konkreter Fallpläne berechnet.

## Vergleich und Schlussfolgerungen

| Projektgruppe | Belegter Stand | Wichtigste Lücke / Konsequenz |
| --- | --- | --- |
| Vereins-Plug-ins | 12 von 14 mit Moodle-CI; Semgrep automatisch bei videolearn | Keine eigenen Testdateien in den gelesenen Vereinsständen; negative Zugriffstests ergänzen |
| Vereins-Dashboard | Externes Python-/Flask-Tool; keine sichtbare GitHub-CI | Eigener Web-/Token-/Abhängigkeits-Prüfpfad erforderlich |
| MooGPT | Externes Node-/Express-Tool mit Tests | Keine sichtbare CI am HEAD; Authentifizierung und Origin-Abgleich vorrangig überprüfen |
| Kurspilot – native Linie | CI am geprüften HEAD erfolgreich; konkrete negative OAuth-, Zugriffs-, Datei- und Netzwerkfälle | Sicherheitsfälle und Gate-Umfang dokumentieren; ältere Mirror-Linie getrennt bewerten |
| ByCS | Unterschiedliche CI-Familien; gemeinsame Vorlage bei 18 Plug-ins; echte negative Tests in der KI-Suite | Öffentliche Konfiguration bildet interne Prüfungen nicht vollständig ab; konkrete negative Testfälle als Referenz nutzen |

**Analyse:** Die größte gemeinsame Verbesserung sind konkrete Tests, die unberechtigte Zugriffe und gefährliche Eingaben zuverlässig ablehnen. Zusätzliche Scanner helfen, brauchen aber selbst geprüfte Regeln und bewertete Befunde. Kurspilot und die bayerische KI-Suite liefern dafür nachweisbare Beispiele.

Ein belegbarer höherer Standard entsteht durch zusätzliche geprüfte Schutzwirkungen und dokumentierte Ergebnisse. Die Anzahl an Tools oder grüne Lint-Läufe allein erlaubt diese Aussage nicht. Ein direkter Vergleich mit dem bayerischen Betriebsstandard benötigt außerdem deren interne Prüf- und Freigabenachweise.

## Empfohlene Reihenfolge

1. MooGPT-Authentifizierung und Origin-Prüfung mit negativen Tests absichern; Tests in CI ausführen.
2. Vereins-CI am jeweiligen Release-Commit nachvollziehen, fehlende Pipelines und inkompatible DB-Matrizen bearbeiten.
3. Je Projekt die anwendbaren Basis- und Funktionsfälle festlegen; eigene Rechte-/Objektgrenzen und Schreiblogik gezielt negativ testen. Korrekte Nutzung gemeinsamer Moodle-APIs im Review nachweisen.
4. Eigene Semgrep-Regeln mit unsicheren/sicheren Fixtures überprüfen; Scanner und Abhängigkeits-/Secret-Prüfungen nach Nutzen und Risiko ergänzen.
5. Gemeinsamen Prüfstandard beschließen; Fortschritt als belegte/anwendbare Fälle ausweisen und je Release Lauf-/Paketnachweise sowie Befunde festhalten.

Die Bestandsaufnahme ändert keine anderen Projekte. Sie ersetzt weder ein vollständiges Codeaudit noch eine institutionelle Freigabe. Es wurden vorhandene Konfigurationen, Testinhalte und ausgewählte veröffentlichte Laufnachweise gelesen; die fremden Tests wurden hier nicht neu ausgeführt. Jede Detailtabelle verlinkt ihre Quellen und beschreibt ihre Grenzen.

## Vorhandene Vorlagen

- [moodle-ci.yml](moodle-ci.yml): Vorlage für Qualität, Installation und Regressionstests; im Repository-Root wird sie nicht automatisch als GitHub-Workflow ausgeführt.
- [moodle-security.yml](moodle-security.yml): 21 Moodle-spezifische Semgrep-Regeln; Abdeckung und bekannte Grenzen stehen in der Vereinsanalyse.

Die Vorlagen sind getrennt von den tatsächlich in den Projekt-Repositories eingebundenen Checks zu bewerten. Ein PHPUnit-Aufruf ohne eigene Testfälle und ein kopierter Regelsatz ohne Ausführung sind keine zusätzlichen Testnachweise.
