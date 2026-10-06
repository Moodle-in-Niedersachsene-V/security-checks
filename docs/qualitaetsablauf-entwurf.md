# Ergänzender Qualitätsablauf für wartbaren Code

Stand: **06.10.2026**. Vorschläge aus dem Gespräch, inspiriert von Clean-Code-Prinzipien; keine überprüfte wörtliche Zuschreibung an Robert C. Martin und keine Moodle-/NLQ-Pflichtliste. Dieses Dokument konkretisiert Kandidaten für gemeinsame Abläufe. Es installiert noch keine Hooks, Werkzeuge oder Skills in den untersuchten Projekten.

Ziel: Änderungen, einschließlich KI-generiertem Code, nachvollziehbar prüfen und die spätere Übernahme erleichtern. Der Ablauf ergänzt [B1 des technischen Prüfstandards](pruefstandard-entwurf.md) sowie [W3/W6 zur Wartbarkeit](nlq-betrieb-und-wartbarkeit.md). Qualitätsmetriken liefern Hinweise auf Pflegebedarf; sie erhalten keine zusätzlichen Punkte im Sicherheitsfortschritt und ersetzen keine Sicherheitsfälle.

## Einordnung der neun Vorschläge

| Kandidat | Gemeinsamer Kern | Projektabhängige Umsetzung |
| --- | --- | --- |
| 1. Ratsche statt fester Schwelle | Keine neuen Regelverstöße; neue/geänderte Logik erfüllt die vereinbarten Regeln. Bekannte Altbefunde explizit erfassen. Gesamtwerte als Trend. | Vergleich pro betroffener Einheit mit festgelegter Basis und gleichem Messumfang. Regeln und begründete Framework-Ausnahmen bestimmen. |
| 2. Gestufte Gates | Schnelle Commitprüfung, vollständige vereinbarte Pushprüfung, gleiche Befehle in verbindlicher CI. Native Git-Hooks als gemeinsamer lokaler Einstieg. | Stack, Testumgebung und Betriebssystem bestimmen Befehle. Umfang der Standardsuite festlegen; teure Mutation und zusätzliche Plattformmatrizen separat ausführen. |
| 3. Fehlschlagslog | Wiederkehrende, normalisierte Fehler sichtbar machen; Häufigkeit und Auswirkung geben Anlass zum Aufräumen. | Ablage, Aufbewahrung und Verantwortlichkeit festlegen; Laufwiederholungen zusammenfassen, Infrastrukturfehler getrennt behandeln. |
| 4. Coverage und CRAP | Testziel explizit zuordnen, echte Produktfunktionen prüfen und vorhandene Berichtswerte nutzen. | PHPUnit-/Coverage-Version, Quellfilter und Treiber erfassen; für Node/Python entsprechende vorhandene Berichte statt PHP-Vorgaben verwenden. |
| 5. Aufteilbare Mutation | Geänderte kritische Logik gezielt auf schwache Tests prüfen; Ergebnisse in isolierten Läufen sammeln. Tests niemals automatisch löschen. | Geeignetes Werkzeug und Instanzisolation nachweisen; Laufteile und Wiederaufnahme an identischen Code-/Konfigurationsstand binden. |
| 6. Schichtregeln per Klassenliste | Tatsächliche Abhängigkeiten und erlaubte Framework-Einstiege prüfen. | Klassen-/Funktionslisten oder andere belastbare Zuordnung statt erzwungener Umbenennung; Regeln nur dort, wo eine echte Schichtung vorhanden ist. |
| 7. Kern und Projektteil | Gemeinsame Review-Regeln knapp halten; Projektverträge und Ausnahmen ergänzen. | Automatisch erzwungene Standards in Werkzeugkonfiguration pflegen und aus der Textdokumentation verlinken. |
| 8. Sichtbare Hook-Fehler | Erforderliche Prüfungen geben Fehlerstatus weiter; übersprungene Prüfungen sind sichtbar. | Setup prüft wirksamen Hook-Pfad, Ausführbarkeit, Werkzeuge und absichtlich ausgelösten Fehler. |
| 9. Eigene ergänzende Skills | Messung und gezielte Testhärtung als getrennte, wiederholbare Schritte beschreiben. | Erst an einem repräsentativen Projekt erproben; eigene Skills anbinden, vorhandene Matt-Pocock-Skills unverändert lassen. |

## Ratsche mit belastbarer Vergleichsbasis

Zu Beginn Quell-Commit, Regel-/Werkzeugversionen, Messumfang und vorhandene Befunde erfassen. Der Vergleich erfolgt gegen eine festgelegte gemeinsame Basis, etwa den Merge-Base-Commit. Neue Verstöße blockieren; Ausnahmen sind fallbezogen und werden nicht automatisch zur neuen Normalität. Bestehende, im Diff berührte Verstöße beheben oder eine nachvollziehbare Ausnahme mit Zuständigkeit festhalten. Kein pauschaler Umbau unberührten Altbestands.

Bei messbaren Eigenschaften wie Komplexität, CRAP oder Testabdeckung zuerst gleiche Einheiten und gleiche Einstellungen vergleichen. Ein Refactoring, neue Codeeinheiten oder geänderte Zuordnung können Vergleichswerte verändern; dann die Änderung erklären statt einen unpassenden globalen Prozentvergleich zu erzwingen. Ein besserer Gesamtwert darf einen neuen lokalen Verstoß nicht verdecken. Fehlende Messungen als „nicht erhoben“ ausweisen.

Die Ratsche ist ein Vorschlag für Qualitätsentwicklung. Fachliche Korrektheit, funktionierende Tests und bestätigte Sicherheitsanforderungen bleiben eigenständige Bedingungen. Es entsteht keine neue universelle Zahlenhürde für alle Plugins.

## Gemeinsame Befehle, dünne Hooks, verbindliche CI

| Stufe | Zweck und Umfang |
| --- | --- |
| `pre-commit` | Schnelle Syntax-/Format-/Regelprüfung des tatsächlich gestagten Inhalts; gegebenenfalls kleine relevante Tests. Uncommittete Änderungen dürfen kein anderes Ergebnis vortäuschen. |
| `pre-push` | Vollständige vereinbarte Standardsuite für die zu pushenden Änderungen: Qualität, Tests und vereinbarte Regression-/Architekturprüfung. Die richtigen zu pushenden Commits berücksichtigen. |
| CI | Dieselben Prüfkommandos am tatsächlichen Kandidatenstand in reproduzierbarer Umgebung; zusätzliche zugesagte Plattformen und Paketprüfungen dort ausführen. Erforderliche Checks als Merge-/Freigabebedingung einrichten. |
| Gezielte Härtung | Mutation oder besonders teure Integrationstests an betroffenen kritischen Schutz-/Fachgrenzen; aus Befunden und Risiko ausgelöst. Vereinbarte Pflichtfälle bleiben vor Freigabe erforderlich. |

Prüfkommandos einmal im Projekt definieren; Hooks und CI rufen sie auf. Ein versioniertes Hook-Verzeichnis kann pro Clone über `core.hooksPath` aktiviert werden. Damit verwenden Menschen, Claude und Codex dieselben lokalen Einstiegspunkte ohne notwendige Husky-Abhängigkeit. Das Verzeichnis allein aktiviert die Hooks nicht; lokale Konfiguration, Ausführbarkeit und Verfügbarkeit der Werkzeuge prüfen. Vorhandene Hooks nicht unbemerkt verdrängen.

Git dokumentiert Hook-Pfad, Ausführbarkeit und Fehlerstatus; lokale Hooks lassen sich umgehen. CI allein ist ebenfalls erst verbindlich, wenn der Ablauf erforderliche erfolgreiche Checks tatsächlich voraussetzt. [Git-Hook-Dokumentation](https://git-scm.com/docs/githooks).

Ein erforderlicher Testbefehl mit `|| true` darf nicht still erfolgreich erscheinen. Fehlerstatus erhalten, auch bei Logausgabe oder Pipelines. Optional nicht blockierende Prüfungen ausdrücklich so benennen; deren Fehlschlag beziehungsweise Auslassen sichtbar melden. Im Setup mit einem absichtlich fehlschlagenden harmlosen Prüfkommando kontrollieren, dass das Gate tatsächlich blockiert. Eine Textsuche nach `|| true` ist ein Hinweis, kein vollständiger Wirksamkeitstest.

## Coverage mit klarer Zuordnung; CRAP aus vorhandenem Bericht

Für PHPUnit die Testziele mit den zur eingesetzten Version passenden Covers-Metadaten angeben. Unterstützende, mitbenutzte Klassen davon unterscheiden. Covers beschränkt den Beitrag eines Tests zur Coverage; es definiert nicht allein den gesamten Nenner. Eigenen Produktionscode im Quellfilter festlegen und ungetestete Dateien im Bericht behalten. Ausnahmen wie `CoversNothing` oder Coverage-Ignores begründen, damit Abdeckung nicht durch Weglassen steigt. Die Regeln versionsgerecht umsetzen; Moodle legt die verwendete PHPUnit-Version mit fest. [PHPUnit-Coverage](https://docs.phpunit.de/en/12.5/code-coverage.html), [Covers-/Uses-Attribute](https://docs.phpunit.de/en/12.5/attributes.html).

CRAP kombiniert Komplexität und Coverage einer Codeeinheit. PHPUnit/php-code-coverage kann es bereits berichten; einen passenden vorhandenen Bericht, etwa HTML oder Crap4J, auswerten. Nicht jedes Exportformat enthält CRAP. Fehlt der Wert, „nicht erhoben“ melden statt einen Wert zu erfinden oder ein zusätzliches Messwerkzeug allein dafür einzuführen. Bericht, Einheit, Version und Messumfang mit erfassen. [PHPUnit-Metriken und Berichtsformate](https://docs.phpunit.de/en/12.5/code-coverage.html).

Diese Angaben gelten für den PHP-Pfad. Keine Covers-/CRAP-Pflicht für andere Stacks ableiten. Coverage der zugeordneten Tests, tatsächliches Testverhalten und Mutationsbefunde bleiben unterschiedliche Nachweise. Hohe Coverage beweist keine starken Assertions.

## Mutation sicher aufteilen und wiederaufnehmen

Zuerst die unveränderte Testsuite erfolgreich ausführen. Mutationsumfang mit Basis-/Kandidaten-Commit und expliziter Fall-/Dateiauswahl festlegen. Für Moodle je parallelem Lauf eigene Testdatenbank, Datenverzeichnisse und notwendige Dienste nutzen; separate Codeverzeichnisse allein isolieren DB-Zustand nicht. Keine Produktionsinstanz verwenden.

Infection unterstützt Diff-Auswahl, Parallelisierung und einzelne Mutanten-IDs; die Dokumentation warnt vor Fehlbefunden bei abhängigen Tests beziehungsweise gemeinsamem DB-Zustand. [Infection-Optionen](https://infection.github.io/guide/command-line-options.html). Das belegt noch keine fertige Wiederaufnahmelösung für unsere Projekte.

Für Wiederaufnahme ein Laufmanifest vorschlagen: Commit, Konfigurations-/Werkzeugstand, Test-/Coverage-Auswahl, Laufteile und abgeschlossene Ergebnisse. Nur gleiche Eingaben zusammenführen; nach relevanten Änderungen betroffene Teile neu ausführen. Überlappungen, fehlende Teile und Abbrüche sichtbar machen. Timeouts, Infrastrukturfehler, überlebende und fachlich äquivalente Mutanten getrennt bewerten; nicht jeden Fehler als erfolgreich getöteten Mutanten zählen.

Schwache Assertions oder fehlende Fälle verbessern. Ein Mutationslauf rechtfertigt keine automatische Testlöschung. Möglicherweise redundante Tests mit Begründung, verbleibender Verhaltensabdeckung und Auswirkungen als Kandidatenliste zur Sammelfreigabe vorlegen. Bis zur ausdrücklichen Freigabe bleiben sie bestehen.

## Schichten und Coding Standards

Moodle und andere Frameworks geben Klassen-/Funktionsnamen und Einstiegspunkte vor. Architekturprüfung deshalb an tatsächlichen Abhängigkeiten, expliziter Zuordnung und erlaubten Ausnahmen ausrichten. Eine Klassenliste muss neue beziehungsweise unbekannte Klassen melden; sonst können neue Verstöße unbemerkt außerhalb der Liste liegen. Globale Funktionen, Callbacks und External-Funktionen bei Bedarf ebenso zuordnen. Nicht allein anhand eines Namens behaupten, eine Schichtregel sei geprüft.

Gemeinsamer Textkern: Regeln, die Review erfordern, etwa Verantwortung von Modulen, erlaubte Abhängigkeiten und verständliche fachliche Verträge. Projektteil: Framework-Einstiege, Zuordnungslisten, zugelassene Ausnahmen und relevante Befehle. Formatierung und andere maschinenprüfbare Standards haben ihre maßgebliche Definition in der Werkzeugkonfiguration; die Dokumentation verweist darauf und dupliziert sie nicht.

## Fehlschläge als Anlass zum Aufräumen

Pro Fehlschlag Prüfschritt, normalisierte Befund-ID, betroffene Einheit, Commit, Ausgang und Beleg erfassen. Wiederholungen derselben Ausführung zusammenfassen; sensible Testdaten und Tokens nicht im zentralen Log speichern. Eine einfache Liste vorhandener CI-Befunde genügt als Start, ohne neues Dashboard.

Rangfolge nach Auswirkung, Häufigkeit und Pflegeaufwand bilden. Wiederkehrende Befunde lösen eine konkrete Aufräumaufgabe aus; kritische Einzelbefunde werden sofort behandelt. Das ist keine Rangliste von Personen. Dieser bedarfsorientierte Ablauf ergänzt die institutionelle Bestands-/Supportprüfung, deren Termine weiterhin gelten.

## Kandidaten für ergänzende Skills

| Kandidat | Eingabe | Ergebnis / Anschluss |
| --- | --- | --- |
| `/baseline-audit` | Projektprofil, festgelegter Commit, vorhandene Prüfkommandos und Berichte | Messumfang, Altbefunde, Werkzeugstände und belastbare Ausgangswerte; nicht verfügbare Metriken ausdrücklich offen lassen |
| `/harden` | Diff-Basis und Kandidatenstand, relevante Risiken, Testumgebung, Ausgangsnachweise | Gezielte Mutationsbefunde, verbesserte Tests, Laufmanifest und erneute Standardprüfung; keine automatische Testlöschung oder Baseline-Absenkung |

Vorgeschlagener Anschluss: Vor Implementierung vorhandene Baseline lesen beziehungsweise erheben; bei relevanter geänderter Logik gezielt härten; im Review Diff, Nachweise und Ausnahmen prüfen. Diese Aufgaben können später aus einem eigenen Ablauf für `/implement` und `/code-review` aufgerufen werden. Die Namen sind hier Skill-Kandidaten, keine Behauptung, dass diese Skills oder Anschlussstellen schon eingerichtet sind.

## Schlanke Einführung

1. Ein repräsentatives Moodle-Projekt als Pilot wählen: vorhandene Befehle, Altbefunde und Testumgebung erfassen.
2. Dünne Git-Hooks und CI mit denselben Befehlen verbinden; nachgewiesene Fehlerweitergabe und erforderliche CI-Checks prüfen.
3. Diffbezogene Ratsche und Review-Regeln einsetzen; wiederkehrende Befunde sammeln.
4. Covers-/CRAP-Auswertung und Mutation nur ergänzen, wenn der Projektstack sie sinnvoll unterstützt; isolierte Ausführung vor Parallelisierung nachweisen.
5. Nach dem Pilot eigene ergänzende Skills beziehungsweise ein globales Template ableiten.

Für dieses Dokument wurden die verlinkten Werkzeugdokumentationen geprüft. Keine Projekt-Hooks installiert, keine Gates in fremden Repositories verändert und keine Mutation ausgeführt.
