# Sicherheitschecks der Vereinsprojekte

Stand: **05.10.2026**. Erfasst: 15 öffentlich erreichbare Repositories von `Moodle-in-Niedersachsene-V`: 14 Moodle-Plug-ins und ein externes Python-Dashboard. Quellen sind die verlinkten, auf Commit festgelegten Dateistände. Private oder zusätzliche lokale Projekte sind damit nicht vollständig erfasst.

## Vergleichstabelle

„ja“ bedeutet im Workflow konfiguriert. „—“ bedeutet im untersuchten Dateistand nicht gefunden, nicht nachweislich weltweit ungenutzt. Bei PHPUnit/Behat stehen Befehle und vorhandene Testdateien getrennt. Laufstatus bezeichnet GitHub Actions; übersprungene Schritte und fehlende eigene Tests bleiben möglich.

| Projekt / Quelle | Moodle-CI | Mustache | Grunt | Testbefehle / eigene Tests | Security-Scanner | Jüngster sichtbarer CI-Lauf |
| --- | --- | --- | --- | --- | --- | --- |
| [local_nextcloudbackup](https://github.com/Moodle-in-Niedersachsene-V/local_nextcloudbackup/blob/63a0ca9589e03161e1d587cdb677f5c42b40dccb/.github/workflows/moodle-ci.yml) | ja | — | — | — | — | [success, 2026-08-14](https://github.com/Moodle-in-Niedersachsene-V/local_nextcloudbackup/actions/runs/31788369855) (HEAD) |
| [block_kursfilter](https://github.com/Moodle-in-Niedersachsene-V/block_kursfilter/blob/ad60a3222fededea6aaad01724266aea728e49ba/.github/workflows/moodle-ci.yml) | ja | ja | ja | — | — | [success, 2026-09-05](https://github.com/Moodle-in-Niedersachsene-V/block_kursfilter/actions/runs/33967457314) (HEAD) |
| [local_easyadmin](https://github.com/Moodle-in-Niedersachsene-V/local_easyadmin/blob/492cabe8eeb23d755cb16c2e417f74bbdfe790be/.github/workflows/moodle-ci.yml) | ja | ja | — | — | — | [success, 2026-10-03](https://github.com/Moodle-in-Niedersachsene-V/local_easyadmin/actions/runs/37131335282) (HEAD) |
| [moodle_dashboard](https://github.com/Moodle-in-Niedersachsene-V/moodle_dashboard/tree/77958c7329bab933c4dc0db47f595fa0daf54a60) | — | — | — | — | — | — |
| [local_calendarcategories](https://github.com/Moodle-in-Niedersachsene-V/local_calendarcategories/blob/12879fd026de328f08f82ea2796f9b4678a3797a/.github/workflows/moodle-ci.yml) | ja | — | — | — | — | [success, 2026-09-08](https://github.com/Moodle-in-Niedersachsene-V/local_calendarcategories/actions/runs/34211407378) (HEAD) |
| [mod_videolearn](https://github.com/Moodle-in-Niedersachsene-V/mod_videolearn/blob/c178113909d8abe09c127cb6c5dc114aa10272a6/.github/workflows/moodle-ci.yml) | ja | ja | ja | — | Semgrep, blockierend | [success, 2026-08-31](https://github.com/Moodle-in-Niedersachsene-V/mod_videolearn/actions/runs/33426232087) (HEAD) |
| [local_peertubeupload](https://github.com/Moodle-in-Niedersachsene-V/local_peertubeupload/blob/ff10053f8299ce08f0602d42c3427b75600847d9/.github/workflows/moodle-ci.yml) | ja | ja | ja | PHPUnit; keine Testdateien | — | [success, 2026-08-06](https://github.com/Moodle-in-Niedersachsene-V/local_peertubeupload/actions/runs/31090588450) (HEAD) |
| [local_plugininfo](https://github.com/Moodle-in-Niedersachsene-V/local_plugininfo/tree/c785a842c6c280e9a95d12f43a8d6d1eafef97dd) | — | — | — | — | — | — |
| [tiny_textgestaltung](https://github.com/Moodle-in-Niedersachsene-V/tiny_textgestaltung/blob/7ade60983a2401b84ccfa42d0ecea865b5b00484/.github/workflows/moodle-ci.yml) | ja | — | ja | — | — | [success, 2026-08-04](https://github.com/Moodle-in-Niedersachsene-V/tiny_textgestaltung/actions/runs/30910309387) (HEAD) |
| [repository_peertubeoauth](https://github.com/Moodle-in-Niedersachsene-V/repository_peertubeoauth/blob/20f2dff51bba3fc67414c275df02fc01eb3d065a/.github/workflows/moodle-ci.yml) | ja | ja | ja | PHPUnit + Behat; keine Testdateien | — | [success, 2026-09-28](https://github.com/Moodle-in-Niedersachsene-V/repository_peertubeoauth/actions/runs/36425013687) (HEAD) |
| [block_mrbs_nds](https://github.com/Moodle-in-Niedersachsene-V/block_mrbs_nds/tree/5176bb5fdbef96eee4fbc332725b496564d36990) | — | — | — | — | — | — |
| [local_emailclient](https://github.com/Moodle-in-Niedersachsene-V/local_emailclient/blob/4f34510313b760c7fa6c175995d8f5da0a00b75f/.github/workflows/moodle-ci.yml) | ja | ja | ja | PHPUnit + Behat; keine Testdateien | — | [success, 2026-08-07](https://github.com/Moodle-in-Niedersachsene-V/local_emailclient/actions/runs/31153428797) (HEAD) |
| [block_questionfilter](https://github.com/Moodle-in-Niedersachsene-V/block_questionfilter/blob/a7bcf379868cda395b85ab6176f97009f492ed9f/.github/workflows/moodle-ci.yml) | ja | — | ja | — | — | [success, 2026-09-10](https://github.com/Moodle-in-Niedersachsene-V/block_questionfilter/actions/runs/34448070603) (älterer Commit) |
| [repository_nextclouddirect](https://github.com/Moodle-in-Niedersachsene-V/repository_nextclouddirect/blob/2fd672b6e29b4d68a670bc271bdbdae78429f751/.github/workflows/moodle-ci.yml) | ja | — | — | — | — | [success, 2026-08-14](https://github.com/Moodle-in-Niedersachsene-V/repository_nextclouddirect/actions/runs/31796451622) (HEAD) |
| [local_kursassistent](https://github.com/Moodle-in-Niedersachsene-V/local_kursassistent/blob/61300a399d8ba2462db1ea8cbaf9cfe8d4132529/.github/workflows/moodle-ci.yml) | ja | — | ja | — | — | [failure, 2026-09-28](https://github.com/Moodle-in-Niedersachsene-V/local_kursassistent/actions/runs/36387574356) (älterer Commit) |

Alle zwölf Moodle-CI-Workflows enthalten Installation, `phplint`, `phpmd`, `phpcs --max-warnings 0`, `phpdoc --max-warnings 0`, `validate` und `savepoints`. **PHP Mess Detector ist überall mit `continue-on-error: true` nicht blockierend.** Die Matrix enthält Moodle 5.1/PHP 8.3/MariaDB und Moodle 5.2/PHP 8.4/PostgreSQL. Einzelne Datenbankversionen unterscheiden sich.

In keinem der 15 rekursiv gelesenen Dateibäume wurden eigene Dateien unter `test/` oder `tests/` gefunden. Eine deklarierte PHPUnit-/Behat-Zeile belegt daher keine selbst implementierten Sicherheitstests. Die Inventur fand zudem keine Workflow-Schritte für Abhängigkeits-Audits, systematische Secret-Scans oder dynamische Sicherheitstests.

Die vollständigen Commit-IDs, Befehle, Datenbankimages, Privacy-Dateipfade und bis zu fünf jüngsten sichtbaren `main`-Läufe stehen in [verein-inventur.json](verein-inventur.json). Ein grüner Lauf ist nur für seinen verlinkten Commit belegt; Branchschutz und verbindliche Merge-/Release-Gates wurden nicht geprüft.

## Konkrete Lücken und Prüfbedarf

| Priorität | Befund | Nächster sinnvoller Nachweis |
| --- | --- | --- |
| Hoch | Eigene negative Sicherheitstests fehlen in den geprüften Vereinsständen | Je exponiertem Endpunkt unberechtigte Rolle, fremder Kurs/Nutzer, fehlende oder ungültige Sesskey, ungültige Parameter und Dateizugriffe testen |
| Hoch | `local_plugininfo`, `block_mrbs_nds` und externes `moodle_dashboard` ohne sichtbaren GitHub-CI-Workflow | Passende automatisierte Checks einrichten; für Dashboard Python-/Web-/Token-Prüfungen statt Moodle-Lint verwenden |
| Hoch | Semgrep nur bei `mod_videolearn` automatisch eingebunden | Regeln zunächst mit positiven/negativen Fixtures absichern, dann risikogerecht in alle Moodle-Plug-ins übernehmen |
| Hoch | `local_emailclient`, `repository_peertubeoauth` und `local_peertubeupload` kombinieren Moodle 5.2 mit PostgreSQL 14 | Laufdetails prüfen und kompatible DB-Version verwenden; Moodle-Plugin-CI-Changelog nennt PostgreSQL 16 als Voraussetzung für Moodle 5.2 |
| Hoch | Bei `local_kursassistent` ist der jüngste gelesene Lauf fehlgeschlagen; bei `block_questionfilter` und `local_kursassistent` passt der jüngste Lauf nicht zum gelesenen HEAD | Aktuellen Release-Commit gezielt prüfen; die fünf gelesenen Läufe sind keine vollständige Historie |
| Mittel | Testmatrix deckt je Version nur eine Datenbank ab; deklarierte Mindeststände stimmen teilweise nicht mit der Matrix überein | Unterstützte Versionen aus numerischem `requires` und realen Zielsystemen ableiten, Mindestversion testen |
| Mittel | Ohne Abhängigkeits- und Secret-Scans bleiben zusätzliche Komponenten und versehentlich veröffentlichte Tokens außerhalb der sichtbaren CI | PHP-/JS-/Python-Abhängigkeiten und Drittbibliotheken inventarisieren; passende Audits und Secret-Scan einsetzen |
| Mittel | Workflow-Dateien legen keine expliziten `permissions` fest; Actions werden über Versions-Tags eingebunden | Minimale Token-Rechte festlegen, Abhängigkeiten der Build-Kette kontrollieren und aktualisieren |

Quelle zur PostgreSQL-Anforderung: [offizieller Moodle Plugin CI Changelog, 4.5.10](https://moodlehq.github.io/moodle-plugin-ci/CHANGELOG.html). Die grünen GitHub-Gesamtstatus trotz dieser Konfiguration sind ein Grund, die einzelnen Installations-/Testschritte und Logs zu prüfen; hier wird daraus kein reproduzierter Laufzeitfehler behauptet.

Die vorhandenen `version.php`-Dateien zeigen verschiedene numerische Mindeststände (siehe JSON). Kommentare wie „Moodle 5.1“ bei `2025041400` sind daher kein belastbarer Ersatz für die tatsächliche Versionserklärung. Die Vorlage in diesem Repository testet 5.0/5.1/5.2, die zwölf gelesenen Vereinsworkflows dagegen 5.1/5.2.

## Das externe Dashboard

[`moodle_dashboard` README](https://github.com/Moodle-in-Niedersachsene-V/moodle_dashboard/blob/77958c7329bab933c4dc0db47f595fa0daf54a60/moodle-dashboard/README.md) beschreibt ein eigenständiges Flask-/SQLite-Dashboard und einen Collector mit Moodle-Webservice-Tokens. Es ist **kein installierbares Moodle-Plug-in**; das zugehörige `local_plugininfo` ist eines.

Sinnvolle zusätzliche Prüfungen: Zugriffsschutz vor Instanz-/Plugin-Daten, sichere Tokenablage und Log-Redaktion, erlaubte Instanzziele/TLS, Fehler- und Timeout-Verhalten des Collectors, Abhängigkeits-Audit und sichere Bereitstellung des Webservers. Das sind aus der Architektur abgeleitete Prüfvorschläge, keine behaupteten konkreten Schwachstellen.

## Grenzen der vorhandenen Semgrep-Regeln

Die Datei [moodle-security.yml](../../moodle-security.yml) enthält 21 PHP-Regeln. Sie sucht unter anderem nach Superglobal-Zugriffen, SQL-Interpolation, unmaskierter Ausgabe, fehlender Sesskey/Login-Prüfung, Dateipfaden, gefährlichen Funktionen, deaktivierter TLS-Prüfung und Geheimnissen. Der automatische Einbau ist bei [mod_videolearn](https://github.com/Moodle-in-Niedersachsene-V/mod_videolearn/blob/c178113909d8abe09c127cb6c5dc114aa10272a6/.github/workflows/moodle-security.yml) mit `--error` belegt.

Beim Lesen der Regeln fallen folgende Grenzen auf; diese wurden **nicht durch einen ausgeführten Semgrep-Test reproduziert**:

- SQL-Ausnahmen hängen an Variablennamen wie `$insql` und können Treffer allein wegen eines Namens ausnehmen; Herkunft und Parameterbindung werden dadurch nicht bewiesen.
- Die Webservice-Regel nimmt Funktionen mit `require_capability()` oder `has_capability()` aus, obwohl das Fehlen von `validate_context()` ihre erklärte Prüffrage ist. Eine Capability-Prüfung ersetzt die Kontextvalidierung nicht.
- Die Pluginfile-Meldung nennt Login **und** Capability; das Muster prüft dagegen nur einen Login-Aufruf beziehungsweise einen vollständig ablehnenden Handler.
- Die Login-Regel erkennt nur eine bestimmte relative Einbindung von `config.php`. Andere Moodle-Verzeichnistiefen und zulässige Login-APIs brauchen eigene Fixtures.
- XSS-, TLS- und Secret-Muster erfassen nur einzelne Schreibweisen und PHP; JavaScript, gespeicherte Daten, Konfiguration und Git-Historie sind dadurch nicht vollständig abgedeckt.
- Ein bloßer `confirm_sesskey()`-Aufruf kann die Sesskey-Abwesenheitsregel ausnehmen; eine weitere Regel warnt zwar vor ungenutztem Rückgabewert, aber die tatsächliche Kontrollflusswirkung bleibt zu prüfen.
- Die Aussage „Systembefehle grundsätzlich unzulässig“ sollte mit den Moodle-Sicherheitsrichtlinien abgeglichen werden; risikobasierte Prüfung und begründete Ausnahmen bleiben nötig.

Für verlässliche Scanner braucht jede Regel mindestens einen absichtlich unsicheren Treffer und eine sichere Gegenprobe. Hinzu kommen dokumentierte Ausnahmen mit Verantwortlichem und Begründung. Das verhindert, dass Null Treffer als vollständige Sicherheitsfreigabe verstanden werden.
