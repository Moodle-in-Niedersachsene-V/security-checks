# Pilot: Easy Admin

Stand: **06.10.2026**. Ergebnis: Ein begrenzter Sicherheitspilot für Easy Admin ist implementiert und gegen eine echte, isolierte Moodle-Testdatenbank ausgeführt. Neun Tests bestehen; zwei absichtlich eingeführte Regressionen werden erkannt. Keine behauptete Vollprüfung des Plugins und kein neu entdeckter Produktionsfehler.

## Auswahl nach gemessenem Umfang

Gezielte Stichprobe kleinerer Vereinsplugins plus Kurspilot. Gezählt werden physische Zeilen in PHP-Dateien, einschließlich Kommentaren, Leerzeilen und eingebettetem HTML/JS. Ausgeschlossen: Sprachdateien, Tests, Vendor-/Node-Abhängigkeiten und Git-Daten. Das ist kein bereinigter SLOC-Wert und keine Größe des gesamten Produkts. Alle Werte beziehen sich auf die gelesenen Commits in [groessenvergleich.json](groessenvergleich.json), vor Ergänzung der Pilot-Tests.

| Kandidat | PHP-Dateien | PHP-Dateizeilen | Abwägung für den ersten Pilot |
| --- | ---: | ---: | --- |
| [Easy Admin](https://github.com/Moodle-in-Niedersachsene-V/local_easyadmin/tree/492cabe8eeb23d755cb16c2e417f74bbdfe790be) | 46 | **7.114** | Breiter als vermutet: administrative Seiten, Nutzerlöschung und Kurssicherungen. Interessante, klar abgrenzbare Schutzfunktionen; vorhandene Moodle-CI. Gewählt für einen kleinen Teilpilot. |
| [Kursfilter](https://github.com/Moodle-in-Niedersachsene-V/block_kursfilter/tree/ad60a3222fededea6aaad01724266aea728e49ba) | 21 | **1.909** | Kleiner; Suche, Gastpfade, Bewertungen und Kursbackups bringen unterschiedliche Sicherheitsgrenzen mit. Gute spätere Referenz für öffentlichere Funktionen. |
| [Kalenderkategorien](https://github.com/Moodle-in-Niedersachsene-V/local_calendarcategories/tree/12879fd026de328f08f82ea2796f9b4678a3797a) | 26 | **3.780** | Mittlerer Umfang; Kategorien-/Mitgliedschafts- und Eventgrenzen, External-Funktionen und Privacy sind interessant. Geeignet für anschließende Fremdobjekt-/Rechtetests. |
| [Plugininfo](https://github.com/Moodle-in-Niedersachsene-V/local_plugininfo/tree/c785a842c6c280e9a95d12f43a8d6d1eafef97dd) | 4 | **348** | Sehr klein: zwei administrative Lese-Webservices. Einfacher Rechtepilot möglich, bildet aber Schreib-/Datei-/Löschgrenzen kaum ab; keine eigene CI im untersuchten Stand. |
| [Kurspilot, native Linie](https://github.com/matthiasgruenwald/moodle-coursepilot/tree/467c954508291a700215b6cb4c2940ac85d29db4/Plugin/src/local_coursepilot) | 165 | **36.023** | Deutlich größer als diese Stichprobe und bereits umfangreich getestet. Für den ersten kleinen Pilot aufwendiger. Ältere Linie und sonstige Repositoryteile sind hier ausgeschlossen. |

Easy Admin enthält insgesamt 48 PHP-Dateien mit 7.948 Zeilen, wenn die beiden Sprachdateien mitzählen. Es ist rund ein Fünftel so groß wie der native Kurspilot-PHP-Pfad nach dieser Zählweise. Die Auswahl folgt dem begrenzbaren Prüfziel, nicht allein dem kleinsten Repository.

## Implementierung und Übernahme

Im lokalen Checkout `/tmp/security-pilot-local_easyadmin` wurde der Branch `test/security-pilot-easyadmin` erstellt. Commit: `7137118` (vollständiger Commit im Patch). Geändert wurden nur:

- `tests/security_test.php`: neun Tests über echte produktive Plugin-Funktionen und Moodle-APIs.
- `.github/workflows/moodle-ci.yml`: blockierender PHPUnit-Schritt und Push-Auslöser für den Pilotbranch; vorhandene Qualitätschecks bleiben erhalten.

Der Push ins Vereinsrepo wurde von GitHub mit **403: fehlende Schreibrechte für matthiasgruenwald** abgelehnt. Der Branch ist daher nicht im Vereinsrepo veröffentlicht. Der vollständige [Git-Patch](security-pilot.patch) ist hier dauerhaft gesichert. Die GitHub-CI für diese Änderung wurde folglich noch nicht ausgeführt; deren Konfiguration ist kein grüner Laufnachweis. Kein Merge auf den Plugin-Hauptbranch.

Eine berechtigte Person kann im sauberen Plugin-Checkout auf Basis `492cabe8eeb23d755cb16c2e417f74bbdfe790be` den Branch erstellen und den Commit übernehmen:

```bash
git switch -c test/security-pilot-easyadmin 492cabe8eeb23d755cb16c2e417f74bbdfe790be
git am /root/security-checks/docs/pilots/easyadmin/security-pilot.patch
git push -u origin test/security-pilot-easyadmin
```

Den Patchpfad gegebenenfalls an den eigenen Checkout dieses Repositorys anpassen. Anschließend laufen die vorhandenen Qualitätschecks plus PHPUnit für die konfigurierte Moodle-/DB-Matrix. Der lokal ausgeführte Pilot deckt erst die unten genannte Einzelkombination ab.

## Konkreter Pilotplan und Nachweise

| Fall | Erwartete Schutzwirkung | Ergebnis |
| --- | --- | --- |
| EA01 | File-Callback lehnt Kurskontext statt Systemkontext ab | Belegt |
| EA02 | Unbekannter Dateibereich wird nicht ausgeliefert | Belegt |
| EA03 | Öffentliche Design-Bildbereiche erlauben anonyme Abfrage; fehlende Datei wird korrekt abgelehnt | Belegt |
| EA04 | Angemeldeter Nutzer ohne `managecourses` wird vor Backup-Abfrage abgewiesen | Belegt |
| EA05 | Dieselbe Capability schützt ZIP-Sicherungen | Belegt |
| EA06 | Nutzer mit gezielt zugewiesener Capability darf den Backup-Abfragepfad erreichen; fehlende Datei bleibt abgelehnt | Belegt |
| EA07 | Inaktivitätsauswahl schützt Gäste, Admins, neue Konten und Konten mit aktueller letzter/erster Aktivität | Belegt |
| EA08 | Deaktivierte automatische Löschung verändert alte Konten nicht | Belegt |
| EA09 | Gefährlich niedrige konfigurierte Frist wird begrenzt; altes normales Konto wird gelöscht, geschützte und jüngere Konten bleiben erhalten | Belegt |

**Pilotplan: 9/9 Fälle automatisiert belegt.** Das ist ausschließlich dieser Teilplan, keine 100-%-Bewertung von Easy Admin. Weitere Adminseiten, manuelle Löschrequests, Sesskey-/HTTP-Grenzen, tatsächliche Dateiübertragung, XSS-Ausgabe, Privacy, alle zugesagten Plattformen und die Wartungsnachweise sind damit noch nicht vollständig geprüft. Insbesondere EA03/EA06 prüfen Abfrage-/Berechtigungswege mit absichtlich fehlenden Dateien, nicht eine erfolgreiche Binärübertragung.

## Tatsächlich ausgeführte Umgebung

- Moodle **5.1.8**, Build 20261005; Core-Commit `cc0bf17da8b5fbd290faf95b2b14a03d64331923`.
- PHP **8.3.35**, PostgreSQL **16.14**, PHPUnit **11.5.55**.
- Eigene Docker-Testdatenbank und eigene Datenverzeichnisse; keine vorhandene Moodle-Instanz oder Produktivdaten verändert.
- PHPUnit mit `--fail-on-warning`: **9 Tests, 23 Assertions, erfolgreich**, siehe [Original-Lauf](phpunit.log).
- Keine gemessene Code-Coverage; Covers-Metadaten ordnen die Testziele zu, ohne daraus einen Messwert abzuleiten.

Moodle-PHPUnit initialisieren, Plugin unter `public/local/easyadmin` installieren und aus dem Moodle-Root ausführen:

```bash
php -d max_input_vars=5000 vendor/bin/phpunit --fail-on-warning public/local/easyadmin/tests/security_test.php
```

## Macht der Pilot einen Unterschied?

Vorher hatte der untersuchte Easy-Admin-Stand keine eigenen Testdateien und keinen PHPUnit-Schritt. Nachher werden die neun benannten Schutzwirkungen wiederholbar geprüft. Die Schutzlogik selbst wurde nicht geändert.

Um die Aussagekraft zu prüfen, wurde jeweils eine Schutzmaßnahme in einer temporären Kopie entfernt und dieselbe Suite erneut ausgeführt, nacheinander gegen die isolierte Testdatenbank:

| Kontrollierte Regression | Erwartete Erkennung | Tatsächliches Ergebnis |
| --- | --- | --- |
| `require_capability('local/easyadmin:managecourses', $context)` aus dem File-Callback entfernt | EA04/EA05 verweigern grünen Lauf | **2 Fehler**, fehlende erwartete Berechtigungsausnahmen: [Log](gegenprobe-berechtigung.log) |
| Mindestfrist-Begrenzung aus dem Cleanup-Task entfernt | EA09 erkennt die unzulässige Löschung eines erst 40 Tage inaktiven Kontos | **1 Fehler**, Konto ist entgegen Erwartung gelöscht: [Log](gegenprobe-aufbewahrung.log) |

Die temporären Änderungen sind nicht im Plugin-Branch/Patch enthalten. Dies sind zwei gezielte Gegenproben, kein vollständiger Mutationslauf und keine allgemeine Mutationsquote. Ihr Nutzen: Die neuen Tests würden mindestens diese konkreten Sicherheitsregressionen blockieren. Sie beweisen nicht, dass alle denkbaren Umgehungen ausgeschlossen sind.

## Folgeschritt

Patch mit berechtigtem Zugang veröffentlichen und die komplette konfigurierte CI prüfen. Danach diesen Teilplan mit Aufwand und Befunden bewerten und den Easy-Admin-Plan um manuelle Schreib-/Löschaktionen erweitern oder am Kursfilter einen zweiten, andersartigen Pilot durchführen. Wartbarkeit W1–W7 bleibt separat zu belegen. Die erfolgreichen Teiltests erlauben noch keine vollständige Betriebsfreigabe.
