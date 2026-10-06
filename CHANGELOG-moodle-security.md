# Änderungen am Semgrep-Regelsatz moodle-security.yml

Stand: 05.10.2026. Grundlage: annotierte Fixtures unter `tests/fixtures/`,
ausgeführt mit Semgrep 1.179.0 über `python3 tests/run_fixtures.py`.

Ergebnis:
- **Neuer Regelsatz:** besteht alle Fixtures, 37 erwartete Treffer und
  32 Gegenproben über 23 Regeln.
- **Bisheriger Regelsatz:** an denselben Fixtures 15 nicht erkannte Fälle und
  7 Fehlalarme.

## Korrigierte Regeln

| Regel | Problem im bisherigen Stand (an Fixture nachgewiesen) | Änderung |
| --- | --- | --- |
| moodle-external-ohne-kontextpruefung | Schlug praktisch nie an. Eine Methode mit `validate_context()` oder `require_capability()` nahm die ganze Klasse aus; auch eine Methode ganz ohne Prüfung blieb unerkannt. | Prüfung je öffentlicher statischer Methode. `require_capability()` ersetzt `validate_context()` nicht mehr. `*_parameters`, `*_returns` und private/protected Hilfsmethoden sind ausgenommen. |
| moodle-pluginfile-ohne-pruefung | Prüfte nur Login, nicht die Capability. Fehlalarm bei `require_course_login()` mit `has_capability()`. | Meldet fehlende Anmeldung **oder** fehlende Capability. Anerkannt werden `require_login`/`require_course_login` sowie `require_capability` oder `has_capability` in einer Bedingung. |
| moodle-einstiegsdatei-ohne-login | Erkannte nur `require_once(__DIR__ . '/../../config.php')`. Andere Tiefen, `require()` und `dirname(__FILE__)` fehlten. Fehlalarm bei `admin_externalpage_setup()`. Mehrfachtreffer je Datei. | Jede Einbindung von `config.php` wird erkannt, ein Treffer je Datei. Anerkannt werden zusätzlich `require_course_login()` und `admin_externalpage_setup()`. CLI-Skripte sind ausgenommen. |
| moodle-fehlender-sesskey | Ein bloßes `confirm_sesskey();` ohne Auswertung galt als Schutz. Fehlalarm bei ausgewertetem `confirm_sesskey()` und bei `moodleform::get_data()`. | Nur wirksame Prüfungen werden anerkannt. Zusätzliche DML-Methoden sind abgedeckt (`insert_records`, `delete_records_list`, `set_field_select`, `execute`). |
| moodle-capability-ohne-archetypes | Setzte eine feste Reihenfolge voraus. Fehlalarm bei `clonepermissionsfrom` und bei `riskbitmask` an späterer Stelle. | Reihenfolgeunabhängig. |
| moodle-capability-ohne-riskbitmask | Fehlalarm, wenn `riskbitmask` nicht an erster Stelle stand. | Reihenfolgeunabhängig. |
| moodle-echo-unescaped | Erkannte nur `echo $var;`, keine Verkettung und keine Interpolation. | Erkennt Variablen in verketteten und interpolierten Ausgaben. Maskierende Funktionen und Casts sind ausgenommen. |
| moodle-geheimnis-im-code | Nur Variablenzuweisungen. | Zusätzlich Objekteigenschaften und Array-Schlüssel. |
| moodle-sql-interpolation | Ausnahme allein über Variablennamen wie `$insql`, und zwar für die ganze Zeichenkette. Eine zweite, ungeschützte Variable daneben blieb unerkannt. | Unverändert als Heuristik, aber auf WARNING herabgestuft. Die Grenze steht in der Meldung. Nutzereingaben deckt die neue Taint-Regel ab. |
| moodle-gefaehrliche-funktion | Meldung „grundsätzlich unzulässig“. | Meldung risikobasiert: vermeiden, sonst `escapeshellarg()` und Begründung; `eval`/`assert` nie. Muster unverändert. |

## Neue Regeln

| Regel | Zweck |
| --- | --- |
| moodle-sql-nutzereingabe (ERROR) | Datenflussanalyse: Werte aus `required_param()`/`optional_param()` oder Superglobalen, die in SQL-Text oder `*_select`-Bedingungen gelangen. Als bereinigend gelten `get_in_or_equal()`, Integer-Casts und eine Positivlistenprüfung mit `in_array()`/`array_key_exists()`. Grenze: nur innerhalb einer Funktion bzw. Datei. |
| moodle-html-writer-inhalt (INFO) | Variable als Inhalt von `html_writer::tag/div/span/link`. `html_writer` maskiert nur Attribute. Häufig unbedenklich, daher nur INFO als Hinweis für die Sichtprüfung. |

## Abgleich an den öffentlichen Vereins-Plugins

Beide Regelsätze liefen am 05.10.2026 über die 14 öffentlichen
Plugin-Repositories (jeweils aktueller Stand von `main`). Die Treffer sind
**Prüfhinweise, keine bestätigten Lücken**.

| | bisher | neu |
| --- | ---: | ---: |
| Treffer gesamt | 78 | 282 |
| davon `block_mrbs_nds` (Altcode) | 67 | 254 |
| übrige 13 Plugins | 11 | 28, davon 20 INFO (html_writer) |

- **block_mrbs_nds:** Der Zuwachs stammt fast vollständig aus diesem Plugin.
  Es gibt dort 216 ungefilterte Ausgaben, die der alte Regelsatz wegen
  Verkettung und Interpolation nicht erkannt hat. Das Plugin sollte gesondert
  gesichtet werden.
- **block_kursfilter:** Drei Einstiegsdateien ohne Login
  (`backup.php`, `guest_login.php`, `rate.php`) sind laut Kommentar bewusst
  öffentlich. Das ist als Ausnahme zu begründen.
- **Taint-Regel:** In keinem Plugin ein Treffer.
- **Webservice-Regel:** Nach der Korrektur kein Treffer in den echten Plugins.

## Hinweise zum Einsatz in CI

- **Schweregrade:** `semgrep --error` bricht bei jedem Treffer ab,
  einschließlich INFO. Für einen blockierenden CI-Schritt daher die
  Schweregrade begrenzen, zum Beispiel:
  `semgrep --config moodle-security.yml --severity ERROR --error .`
  WARNING und INFO laufen dann als Bericht mit.
- **Regeländerungen:** Nach jeder Änderung `python3 tests/run_fixtures.py`
  ausführen. Neue Regeln brauchen mindestens eine `ruleid`- und eine
  `ok`-Annotation.
- **Bewertung:** 0 Treffer sind keine Sicherheitsfreigabe. Die Grenzen stehen
  jeweils in der Regelmeldung.
