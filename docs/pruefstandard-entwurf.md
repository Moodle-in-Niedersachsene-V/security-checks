# Vorschlag für einen gemeinsamen Prüfstandard

Stand: **05.10.2026, überarbeiteter Entwurf v2**. Keine beschlossene Vereinsrichtlinie, Moodle-Zertifizierung oder bestätigte NLQ-Anforderung. Grundlage: [Moodle-Sicherheitsrichtlinien](https://moodledev.io/general/development/policies/security), [Moodle Plugin CI](https://moodlehq.github.io/moodle-plugin-ci/Help.html) und [Recherche mit etablierten Plugins](research/moodle-pruefstandard-kalibrierung.md).

## Warum der Entwurf vereinfacht wurde

Die zehn gleich gewichteten Bereiche des ersten Entwurfs waren unsere Zusammenstellung. Zusätzliche Scanner erhielten eigene Pflichtpunkte, Release-Nachweise wurden teilweise doppelt bewertet und noch nicht untersuchte Anforderungen blieben pauschal im Nenner. Daraus entstand beispielsweise die irreführende Bewertung des AI Chat mit 35 %. Diese Skala wird zurückgezogen; sie ist im [ursprünglichen Commit](https://github.com/Moodle-in-Niedersachsene-V/security-checks/tree/8d762c0) nachvollziehbar.

Moodle verlangt passende Schutzmechanismen und empfiehlt Tests. Daraus folgt keine allgemeine Pflicht zu einer bestimmten Scannerkette oder einer festen Coverage-Prozentzahl. Unser Mehrwert soll aus gezielten Prüfungen eigener Schutzgrenzen entstehen. Die Recherche unterscheidet Implementierungsrichtlinien, empfohlene Tests und zusätzliche Vereinsanforderungen.

## Vier Basisbereiche

Die Bereiche strukturieren den Plan; sie erhalten keine pauschalen Punkte. Je Projekt werden konkrete, anwendbare Fälle mit erwartetem Ergebnis festgelegt.

| ID | Bereich | Schlanker Ausgangspunkt | Geeigneter Nachweis |
| --- | --- | --- | --- |
| B1 | Qualität und Kompatibilität | Moodle Plugin CI: Installation, PHP-Lint, Moodle-Code-/Dokumentationsprüfung, Struktur und Savepoints; Mustache/Grunt soweit verwendet. Eigene Tests ausführen. Deklarierte und eingesetzte Moodle-/PHP-/DB-Stände berücksichtigen; Upgrade bei eigener Änderung prüfen. | Erfolgreicher Lauf mit geprüften Versionen; begründete Auswahl statt vollständigem Kreuzprodukt aller Plattformen |
| B2 | Anmeldung und Berechtigungen | Login beziehungsweise API-Authentifizierung, richtiger Kontext, Capability und relevante Kurs-/Gruppen-/Nutzer-/Objektgrenzen | Review der Einstiegspunkte; eigener Negativtest mit unberechtigter Rolle und fremder Objekt-ID sowie zulässiger Gegenfall je unterschiedlicher Schutzlogik |
| B3 | Zustandsänderungen | Form-, AJAX- oder Webservice-Schutz passend zur API und Authentifizierung | Review korrekter API-Nutzung; eigene Request-Verarbeitung mit fehlendem/ungültigem Sesskey oder Token prüfen; abgelehnter Request verändert keine Daten |
| B4 | Eingaben und Ausgaben | Typisierte Parameter, gebundenes SQL, passende Text-/HTML-/JS-Ausgabe; externe Inhalte als unvertrauenswürdig behandeln | Review der eigenen Verarbeitung; problematische Zeichen und manipulierte Inhalte an relevanten Eingabe-/Ausgabepfaden testen |

**Praktische Grund-QA:** Die bereits berücksichtigten [Guardians-QA-Prechecks](https://moodledev.io/general/community/plugincontribution/guardians/qaprechecks) empfehlen eine Testinstanz mit Developer Debugging, einem abweichenden Tabellenpräfix und möglichst PostgreSQL. Für B1 übernehmen wir daraus: passende Metadaten und Abhängigkeiten prüfen, sauber installieren, wichtige Moodle-Funktionen auf Regressionen prüfen und die beschriebenen Plugin-Funktionen testen. Debugging bleibt auf die Testumgebung beschränkt. Diese Seite beschreibt ausdrücklich den früheren Directory-Reviewprozess; sie ist eine nützliche Prüfreferenz, keine bestätigte aktuelle Marketplace-Pflichtliste.

**Moodle-APIs richtig prüfen:** Browserseiten und External-Funktionen unterscheiden. Bei External-Funktionen sind Parameter-/Kontextvalidierung und Capabilities maßgeblich; `require_login()` ist dort kein universeller Zusatzcheck. Die [External-API-Dokumentation](https://moodledev.io/docs/5.0/apis/subsystems/external/writing-a-service) beschreibt den Ablauf. Die [Form API übernimmt Sesskey- und Methodenprüfung](https://moodledev.io/general/development/policies/security/crosssite-request-forgery); ihre korrekte Einbindung prüfen, nicht alle Core-Schutzfunktionen im Plugin erneut testen. Nicht jede Leseoperation braucht einen Sesskey.

Review belegt die nachvollzogene Implementierung, ein ausgeführter Test das getestete Verhalten. Kritische eigene Rechte-/Tokenlogik sollte wiederholbare Negativtests haben. Gleichartige Endpunkte können gemeinsam erfasst werden, wenn dieselbe Schutzlogik nachweislich greift; abweichende Grenzen brauchen eigene Fälle. Ein PHPUnit-/Behat-Befehl ohne eigene Testfälle ist kein zusätzlicher Verhaltensnachweis.

## Zusatzmodule nach vorhandener Funktion

| Modul / Auslöser | Passende Prüfung |
| --- | --- |
| Dateien: eigene Uploads, Downloads, `pluginfile` | Kontext, Eigentümer, fremde Datei-ID; Pfadverarbeitung, Typ-/Größenbegrenzung soweit relevant |
| Netzwerk: externe Requests | TLS und angemessene Zeit-/Größenlimits; bei beeinflussbaren Zielen zusätzlich erlaubte Ziele, SSRF und relevante Redirects |
| Personenbezogene Daten | Für jedes Plugin passenden [Privacy-Provider](https://moodledev.io/docs/5.3/apis/subsystems/privacy) prüfen. Bei eigener Speicherung/Weitergabe zusätzlich Metadaten, Export/Löschung, Empfänger und Aufbewahrung; fremde Daten bleiben geschützt. `null_provider` nur bei tatsächlich erfüllten Voraussetzungen. |
| OAuth, Tokens, externes Tool | Serverseitige Identität, Ablauf/Widerruf und Rechtebindung; CSRF/CORS passend zum Authentifizierungsmodell. Origin allein beweist keine Identität. |
| KI/LLM | Berechtigte Daten für Prompt/Antwort, sichere Ausgabe unvertrauenswürdiger Antworten, angemessene Mengen-/Kostenlimits; bei Aktionen erneute Rechteprüfung |
| Eigene Drittbibliotheken | Versionen inventarisieren und bekannte relevante Schwachstellen bewerten; geeignetes Auditwerkzeug verwenden, sofern sinnvoll |
| Backup/Restore, Migration, Löschung | Eigene Daten erhalten beziehungsweise richtig entfernen; Rechte und Eigentümer nach Restore; betroffene Upgradepfade |

Für MooGPT und das Dashboard gelten passende Schutzwirkungen ebenfalls, aber keine Moodle-spezifischen Lint-/Form-API-Vorgaben. Sie betreiben eigene Webschnittstellen und brauchen einen eigenen Nachweis. Bei Kurspilot native Linie und ältere externe/Mirror-Linie getrennt planen; Ergebnisse nicht auf andere Komponenten übertragen. grAIde bleibt ausgeschlossen.

Für die Entwicklung ergänzt der [Qualitätsablauf-Entwurf](qualitaetsablauf-entwurf.md) B1 um diffbezogene Regeln, wirksame lokale Gates und gezielte Testhärtung. Die dortigen Metriken sind Wartungshilfen und fließen nicht als zusätzliche Sicherheitspunkte in den Prüfplan ein.

## Ergänzende Werkzeuge

SAST, Secret-Scanning und Abhängigkeits-Audits können sinnvoll sein. Sie sind **keine jeweils zehn Pflichtpunkte** und keine allgemeine, hier belegte Moodle-Werkzeugpflicht. Fehlende Workflow-Zeilen beweisen auch nicht, dass Plattformscans oder interne Prüfungen fehlen.

Die vorhandenen eigenen Semgrep-Regeln zunächst mit unsicheren Treffern und sicheren Gegenfällen prüfen; danach nach Nutzen und Risiko einsetzen. Scanner-Funde fachlich bewerten und Ausnahmen begründen. Keine universelle Pflicht zu DAST, Live-DNS-Labor, Browser-E2E für jede Funktion, vollständiger Plattformmatrix oder 80 % Liniencoverage. Solche Prüfungen ergänzen, wenn konkrete Funktionen oder Betriebsanforderungen sie rechtfertigen.

## Fortschritt und Freigabe

**Prüfplan-Fortschritt = belegte anwendbare Fälle / alle festgelegten anwendbaren Fälle × 100.** Zähler/Nenner, Planversion und geprüften Commit anzeigen. Status je Fall: **belegt**, **offen**, **fehlgeschlagen**, begründet **nicht anwendbar**. Nicht anwendbare Fälle entfallen aus dem Nenner; offene und fehlgeschlagene bleiben darin. Bei offenen Fällen unterscheiden: noch nicht untersucht oder Nachweis fehlt. Belegart separat: Review, manueller Test, automatisierter Test.

Das ist ein Fortschrittswert für den vereinbarten Plan. Die **automatisierte Testabdeckung des Plans** wird separat als erfolgreich automatisiert geprüfte Fälle / anwendbare Fälle angegeben; Code-Liniencoverage bleibt ein weiterer eigener Wert. Vergleich nur bei hinreichend gleichem Umfang. Keine neuen Prozentwerte ohne festgelegten Fallplan. Beispiele und Bestandsaufnahme: [Prüfplan-Abdeckung](pruefplan-abdeckung.md).

Vor einer Freigabe einmal festhalten: geprüfter Commit und zugeordnetes Paket, vereinbarte erfolgreiche Checks, verbleibende Befunde und verantwortliche Person. Ein Paket-Installationsnachweis gehört an diese Grenze, ohne denselben fehlenden Beleg mehrfach abzuwerten. Bestätigte ausnutzbare Lücken blockieren eine reguläre Freigabe unabhängig vom Fortschrittswert. Nach relevanten Änderungen die betroffenen Fälle erneut prüfen.

```markdown
Projekt / Komponenten / verantwortliche Person:
Prüfdatum / Planversion / geprüfter Commit / zugeordnetes Paket:
Unterstützte und getestete Versionen:

| Fall-ID / Bereich | Konkreter Fall und erwartetes Ergebnis | Status | Belegart / Quelle | Befund oder N/A-Begründung |
| --- | --- | --- | --- | --- |

Prüfplan-Fortschritt: __ / __ anwendbare Fälle (__ %)
Automatisiert erfolgreich geprüft: __ / __ anwendbare Fälle (__ %)
Offene Befunde / Zuständigkeit / Termin:
Freigabeentscheidung / verantwortliche Person:
```

## Was recherchiert wurde und was noch offen ist

**Gefunden und berücksichtigt:** Moodle veröffentlicht konkrete Sicherheitsrichtlinien, API-Vorgaben und Empfehlungen zu Tests und Plugin CI. Die [Moodle-Recherche](research/moodle-anforderungen.md) beschreibt diese Quellen. Es fehlt also nicht an Moodle-Sicherheitsanforderungen.

**Noch nicht vollständig zugänglich:** Die aktuelle Marketplace-Einreichungsliste war im Recherchezugriff nicht lesbar. Deshalb beansprucht dieser Entwurf keine vollständige Abbildung aller aktuellen Aufnahmebedingungen des Marketplace. Diese begrenzte Quellenlücke verhindert einen eigenen Prüfstandard für den Betrieb beim NLQ nicht.

**Was „Kalibrierung“ bedeutet:** Der erste Entwurf wurde mit Moodle-Richtlinien und konkreten Tests von Attendance, Checklist und ObjectFS abgeglichen. Dabei wurden Implementierungsanforderungen, Testempfehlungen und eigene Zusatzanforderungen unterschieden. Ergebnis: vier Basisbereiche, Zusatzmodule nach Funktion, keine pauschalen Scannerpunkte und keine doppelte Abwertung desselben Release-Nachweises. Das war eine Überprüfung unseres Maßstabs; die Referenzplugins wurden nicht neu ausgeführt oder als sicher zertifiziert. Einzelquellen stehen in der [Kalibrierungsrecherche](research/moodle-pruefstandard-kalibrierung.md).

**Ziel beim NLQ:** Nach der im Gespräch beschriebenen Ausgangslage entwickelt das NLQ seinen Umgang mit Plugin-Sicherheit und Wartbarkeit noch. Dieses Repository soll dafür einen Vorschlag liefern. Der [Vorschlag für Betrieb und Wartbarkeit](nlq-betrieb-und-wartbarkeit.md) ergänzt die technischen Tests um Betreuung, Übernahmefähigkeit, Supportzeitraum und Entscheidungen über Nachbesserung oder Ablösung. Diese Kriterien sind unser Diskussionsvorschlag und noch keine beschlossenen NLQ-Vorgaben.
