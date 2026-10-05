# Eigene Projekte: Sicherheitschecks

Erhebung: **05.10.2026**. Gelesen wurden ausschließlich `matthiasgruenwald/moo-gpt`, `matthiasgruenwald/moodle-coursepilot` und `matthiasgruenwald/moodle-local_coursepilot`. Andere private Repositories wurden nicht untersucht. Bestandsaufnahme von Quellen und bestehenden CI-Nachweisen; kein neuer Testlauf und kein Penetrationstest.

## Prüfstände und Produktgrenzen

| Projekt/Teil | Fixierter Commit | Einordnung |
| --- | --- | --- |
| MoodGPT (`moo-gpt`) | [`a53f5acf43cedf04d46edf916bbff0545bac89d1`](https://github.com/matthiasgruenwald/moo-gpt/commit/a53f5acf43cedf04d46edf916bbff0545bac89d1), 31.05.2026 | Externes Node/Express-/WebSocket-Tool mit SQLite; Umbau zum Moodle-Aktivitätsplugin ist ein Zukunftsplan, kein aktueller Prüfnachweis. [Roadmap][mg-roadmap] |
| Kurspilot, natives Plugin | [`467c954508291a700215b6cb4c2940ac85d29db4`](https://github.com/matthiasgruenwald/moodle-coursepilot/commit/467c954508291a700215b6cb4c2940ac85d29db4), 01.10.2026 | `Plugin/src/local_coursepilot`: nativer Server-MCP in Moodle; aktuelle 2.0-Linie als Beta. [ADR 0027][cp-adr] |
| Kurspilot, lokales Tool/Altplugin | Derselbe Quellencommit | Lokaler stdio-MCP, Installer und `legacy/local_coursepilot` sind eingefrorener Altstand. Einige Node-Vertragstests prüfen ausdrücklich diese Linie. [ADR 0027][cp-adr], [Datenschutz-Vertrag][cp-dp] |
| Marketplace-Mirror | [`820c7e3293f5bca906808d7727289c55be727024`](https://github.com/matthiasgruenwald/moodle-local_coursepilot/commit/820c7e3293f5bca906808d7727289c55be727024), 23.08.2026 | Eigenständiger älterer Plugin-Stand. Der aktuelle Export baut weiterhin **`legacy/local_coursepilot`**, nicht das native Plugin; automatische Synchronisierung abgeschaltet. [Export][cp-mirror-build], [Workflow][cp-mirror-ci] |

## Konfigurierte Checks und Ausführungsnachweise

| Bereich | MoodGPT | Kurspilot: natives Plugin/Quellenrepo | Marketplace-Mirror |
| --- | --- | --- | --- |
| Automatischer CI-Trigger | Keine `.github/workflows` im untersuchten vollständigen HEAD-Dateibaum | Push auf `main`/`dev`, Pull Requests, manuell; `contents: read` [Workflow][cp-ci] | Keine Workflow-Dateien im untersuchten vollständigen HEAD-Dateibaum |
| Unit-/Integrationstests | `npm test` → Node-Test-Runner; 50 Testdateien im Baum. Kein Actions-Lauf am HEAD gefunden [package.json][mg-package] | PHPUnit auf Moodle 5.0 und 5.1, PHP 8.4, MariaDB 11; Node-Testlauf und Syntaxcheck des AMD-Moduls [Workflow][cp-ci] | Keine Testdateien im Mirror-Dateibaum |
| Coverage | Kein konfiguriertes Coverage-Gate gefunden | Mindestwert 80% auf Moodle 5.0 für native PHP-Produktionsdateien; Log: **80,90% (10.944/13.527 Zeilen)** [Coverage-Job][cp-coverage-job] | Kein eigener Nachweis; native Coverage gilt nicht für Legacy-Mirror |
| Syntax und Moodle-Prechecks | Kein automatisches Lint-Gate gefunden | `moodle-plugin-ci phplint`; keine Aufrufe von `phpcs`, `phpdoc`, Mustache-/Grunt-Checks im untersuchten Workflow [Workflow][cp-ci] | Export prüft Pflichtdateien, Komponente, GPL-Lizenz und `.DS_Store`; dies sind Paket-/Metadatenchecks [Export][cp-mirror-build] |
| Release-Paket | Kein automatisches Paket-Sicherheitsgate gefunden | ZIP bauen, SHA-256 protokollieren, frische Moodle-Installation aus genau diesem ZIP; Registrierung, Discovery, OAuth-/Toolaufrufe sowie 401 ohne Token [Artefaktprüfung][cp-artifact] | Export und manueller Sync; kein CI-Test des Mirror-Pakets belegt [Workflow][cp-mirror-ci] |
| Browser/E2E | Kein automatisierter Browser-Sicherheitslauf gefunden | Playwright-Skript vorhanden; `native-ci.yml` ruft es nicht auf. Ohne `.env.e2e` werden Moodle-abhängige Specs übersprungen [package.json][cp-package], [Testregeln][cp-testing] | Kein eigener Browser-Testnachweis |
| SAST, Secrets, Abhängigkeiten | Keine Semgrep-/CodeQL-/Gitleaks-/Audit-Workflows im untersuchten Baum gefunden | Keine entsprechenden Workflows oder Audit-Schritte im untersuchten Baum/CI gefunden | Keine entsprechenden Workflows gefunden |
| Ausgeführt am fixierten HEAD | Actions-API-Abfrage ergab **0 Läufe**; lokale Ausführung bleibt unbekannt | Zwei erfolgreiche Push-Läufe am 01.10.2026. Näher geprüft: [Lauf 36885192594][cp-run]; PHPUnit-Matrix, JS, Artefakt und Gesamtgate erfolgreich | Actions-API-Abfrage ergab **0 Läufe** |

Der näher geprüfte Kurspilot-5.0-Job meldet **1.321 Tests, 127.775 Assertions, 1 Skip**. JS: **101 Testdateien**, sechs ausdrücklich ausgeschlossene Dateien (OS-Credential-Store, lokale Start-/Uninstall-Schritte und einzelne Altstandtests). Die Rohzahl umfasst Funktionalitäts- und Vertragstests; sie ist keine Zahl geprüfter Sicherheitsanforderungen. [PHPUnit-Job][cp-coverage-job], [JS-Job][cp-js-job], [Ausschlussliste][cp-js-runner]. „Gate (required)“ ist der Workflow-Name; eine tatsächlich eingerichtete Branch-Protection wurde nicht verifiziert.

## Inhaltlich geprüfte Sicherheitsfälle

Die folgenden Aussagen beruhen auf gelesenen Assertions und Aufrufen, nicht nur Dateinamen. Der erfolgreiche Gesamtlauf belegt die Suite-Ausführung; nicht für jeden einzelnen Fall wurde ein separater Ergebnisbericht ausgewertet.

| Projekt/Teil | Nachgewiesener Testinhalt | Grenze des Nachweises |
| --- | --- | --- |
| MoodGPT: Admin-/Lehrkraftauth | HTTP-Anfragen an den echten Router: ohne/mit ungültigem Token 403, Nicht-Admin an Admin-Endpunkt 403 [Admin-Tests][mg-admin-test] | Testserver und Testdatenbank; kein Nachweis der Moodle-Identitätsbindung im Produktivbetrieb |
| MoodGPT: WebSocket-Origin | Aufruf der produktiven Middleware: unerlaubte Domain führt zu Fehlerframe und Close 1008 [Origin-Tests][mg-origin-test] | Kein Test einer Domain, die mit erlaubter Origin beginnt; ohne `ALLOWED_ORIGIN` wird Zugriff ausdrücklich erlaubt |
| MoodGPT: Rate-Limit | Produktiver Limiter: Überschreitung stoppt Verarbeitung und schließt WS; IPs zählen getrennt [Rate-Tests][mg-rate-test] | Ohne `MAX_REQUESTS` unbegrenzt. Der vermeintliche Datumswechseltest erzeugt nur einen neuen Limiter und prüft keinen echten Tageswechsel |
| MoodGPT: Markdown/XSS | `<script>` wird in einer Testfunktion maskiert [Markdown-Test][mg-markdown-test] | Browserfunktion wird im Test **nachgebaut**; deshalb kein verlässlicher Regressionstest des produktiven Renderers |
| MoodGPT: Schüler-Memory | Ungültiges Lehrkrafttoken führt zu 403; zugleich GET/POST/DELETE per Schüler-`userId` ohne Token als erfolgreicher Pfad getestet [Memory-Tests][mg-memory-test] | Keine nachgewiesene Sperre beim Zugriff auf die `userId` einer anderen Person |
| Kurspilot nativ: OAuth/PKCE | Fehlende PKCE → `invalid_request`; falscher Verifier → kein Token; zweites Einlösen desselben Codes → abgewiesen. Zusätzlich Ablauf, Redirect-/Client-Mismatch und Refresh-Rotation getestet [OAuth-Tests][cp-oauth-test] | PHPUnit auf Moodle-Testinstanz; kein unabhängiger externer OAuth-Audit |
| Kurspilot nativ: Personen-/Kursgrenzen | Dispatcher-Test erzeugt zwei Lehrkräfte mit getrennten Kursen und prüft explizit, dass A Kurs B nicht sieht; abgelaufene/widerrufene Tokens, fehlende Rechte, Kill-Switch und unerlaubte Origins getestet [Dispatcher-Tests][cp-dispatcher-test] | Weitere Rollen/Endpunkte müssen anhand einer vollständigen Rechtefallmatrix bewertet werden |
| Kurspilot nativ: Remote-Freigabe | Kohortenfreigabe öffnet keinen fremden Kurs; Entzug der Kohortenmitgliedschaft blockiert bestehende Verbindung, Entzug der Kurseinschreibung blockiert Kurswerkzeug [Remote-Tests][cp-remote-test] | Keine pauschale Aussage für sämtliche vorhandenen externen Funktionen |
| Kurspilot nativ: SSRF/Transport | Moodle-URL-Sicherheitshelfer blockiert Testhost; Fehlermeldung enthält Testpasswort nicht. TLS-Verifikation, keine Redirects, Timeout und Größenlimit in Transportoptionen explizit geprüft [WebDAV-Tests][cp-webdav-test] | Blockierter Host über kontrollierten Helfer simuliert; kein Live-Test aller DNS-/Netzwerkumgehungen |
| Kurspilot nativ: Downloadtickets | Zweite Nutzung, Ablauf, veränderte Inhalte, widerrufene Verbindung sowie gesperrte/gelöschte Accounts werden abgewiesen [Ticket-Tests][cp-ticket-test] | Testfälle sind kein allgemeiner Nachweis für alle Datei-Downloadwege |
| Kurspilot nativ: Privacy | Export enthält keine Klartexttokens; Löschung entfernt die Bestände der Zielperson und erhält diejenigen einer anderen Person; WebDAV-Datentransfer als Metadatum [Privacy-Tests][cp-privacy-test] | Technischer API-Test, keine rechtliche Datenschutzfreigabe |
| Kurspilot nativ: Pfade | `..`/`.` und Traversal abgewiesen, Dateiverwaltungsfähigkeit geprüft, aktuelle Person bestimmt Kontext [Kontextdatei-Tests][cp-context-test] | Kein vollständiger Live-Test von Symlinks oder Betriebssystem-Dateirechten |
| Kurspilot nativ: Integrität | Drift sperrt betroffenen Aktivitätstyp; fehlerhafter XML-Import hinterlässt keine geschriebenen Fragen [Write-Gate][cp-write-test], [XML-Importtests][cp-import-test] | XML-Negativfälle nicht mit einem vollständigen XXE-/Parser-Fuzzing gleichsetzen |
| Kurspilot Altlinie: Datengrenze | Node-Vertrag prüft exakte Tool-/Webservice-Allowlist sowie Ausschluss von Lernendendaten-Werkzeugnamen; einzelne Prüfungen sind Quelltext-/Dokumentationsregex [Datenschutz-Vertrag][cp-dp] | `PLUGIN_ROOT` verweist auf **`legacy/local_coursepilot`**; dieser Vertrag darf nicht als Privacy-Nachweis des nativen Plugins gelten |
| Kurspilot lokales Tool | Produktions-Pfadfunktionen werden mit Traversal und Zielen außerhalb der Arbeitsbereichwurzel aufgerufen; erwartet wird eine Exception [Pfadtests][cp-local-path-test] | Lokale Altlinie; unabhängig von den nativen Moodle-Dateitests bewerten |

## Priorisierte Lücken und nächste Nachweise

1. **MoodGPT: verbindliche Identitätsprüfung und Origin-Grenzen.** Der Schüler-Memory-Router verwendet frei übergebene `userId` für Lesen/Schreiben/Löschen; davor steht lediglich `isOriginAllowed`. Diese Funktion sowie WS-Auth akzeptieren `origin.startsWith(erlaubteOrigin)`. Damit würde beispielsweise `https://moodle.example.com.evil.example` die erlaubte Zeichenfolge `https://moodle.example.com` erfüllen. Quellbefund, kein durchgeführter Produktionsangriff. Nötig sind negative Tests für fremde Schüleridentitäten, manipulierte Origin/Referer und exakte URL-Origin-Grenzen sowie eine serverseitig belegte Identität. [Router][mg-memory], [Auth][mg-auth].
2. **MoodGPT: Tests automatisch ausführen.** Vorhandene Auth-/Rate-Tests in CI aufnehmen; nachgebaute Renderer-/postMessage-Logik durch Tests der produktiven Funktionen ergänzen. Erst danach sind erfolgreiche Läufe als wiederholbarer Nachweis verfügbar. [package.json][mg-package], [Markdown-Test][mg-markdown-test].
3. **Kurspilot: geprüften Release und Mirror zusammenführen.** Native CI weist das native ZIP nach, während Marketplace-Mirror noch Legacy exportiert. Bei Umstellung Commit und ZIP-Prüfsumme dokumentieren und genau den veröffentlichten Stand gegen dieselben Gates testen. [ADR 0027][cp-adr], [Export][cp-mirror-build].
4. **Gemeinsam: Scanner und Paketabhängigkeiten.** In den untersuchten Konfigurationen fehlen automatische Secrets-/Abhängigkeitsscans und dedizierter Sicherheits-SAST. Das ist kein automatisch nachgewiesener Verstoß gegen Moodle-Anforderungen. Eigene Abhängigkeiten und Risiken bestimmen passende Ergänzungen; bereits manuelle oder plattformseitige Scans sind damit weder belegt noch ausgeschlossen. Siehe [Kalibrierung](moodle-pruefstandard-kalibrierung.md).
5. **Kurspilot: zusätzliche Plattformen und echte HTTP-/Browsergrenzen.** Tatsächlich zugesagte DB-/Moodle-Versionen berücksichtigen; eigene Login-/CSRF- und HTTP-/OAuth-/Dateigrenzen gezielt prüfen. Keine zusätzliche DB-Pflicht allein für eine größere Matrix. Die vorhandene CI verwendet MariaDB und ruft Playwright nicht auf. Sechs ausgeschlossene OS-/Altlinientests benötigen einen gesonderten Nachweis, falls die Altlinie noch verteilt wird. [Workflow][cp-ci], [Ausschlussliste][cp-js-runner], [Testregeln][cp-testing].

## Quellen und Abrufgrenzen

- Alle Quelltextlinks sind commitfixiert; Actions-Links beziehen sich auf konkrete Läufe/Jobs. Keine Aussagen aus unverifizierten Badges übernommen.
- Dateibäume wurden vollständig gelesen (`truncated: false`). Abwesenheit bezieht sich auf diese drei HEAD-Stände, nicht auf andere Branches, externe CI oder unveröffentlichte Arbeiten.
- Actions-Abfragen: `GET /repos/matthiasgruenwald/<repo>/actions/runs?head_sha=<oben genannter SHA>&per_page=20`; Kurspilot zusätzlich Jobstatus und Logs des angegebenen Laufs. Keine Tests selbst gestartet; Konfiguration und Ausführung bewusst getrennt.
- Die Stichprobe umfasst konkrete Sicherheitsfälle, keine vollständige Codeprüfung aller Funktionen. „Nicht nachgewiesen“ ist kein Beweis für Unsicherheit und kein Beweis für vorhandenen Schutz.

[mg-roadmap]: https://github.com/matthiasgruenwald/moo-gpt/blob/a53f5acf43cedf04d46edf916bbff0545bac89d1/docs/moodle-plugin-roadmap.md
[mg-package]: https://github.com/matthiasgruenwald/moo-gpt/blob/a53f5acf43cedf04d46edf916bbff0545bac89d1/package.json
[mg-admin-test]: https://github.com/matthiasgruenwald/moo-gpt/blob/a53f5acf43cedf04d46edf916bbff0545bac89d1/test/admin-route.test.js
[mg-origin-test]: https://github.com/matthiasgruenwald/moo-gpt/blob/a53f5acf43cedf04d46edf916bbff0545bac89d1/test/auth-middleware-origin-ws.test.js
[mg-rate-test]: https://github.com/matthiasgruenwald/moo-gpt/blob/a53f5acf43cedf04d46edf916bbff0545bac89d1/test/rate-limiter.test.js
[mg-markdown-test]: https://github.com/matthiasgruenwald/moo-gpt/blob/a53f5acf43cedf04d46edf916bbff0545bac89d1/test/simpleMarkdown.test.js
[mg-memory-test]: https://github.com/matthiasgruenwald/moo-gpt/blob/a53f5acf43cedf04d46edf916bbff0545bac89d1/test/student-memory-route.test.js
[mg-memory]: https://github.com/matthiasgruenwald/moo-gpt/blob/a53f5acf43cedf04d46edf916bbff0545bac89d1/routes/student-memory.js
[mg-auth]: https://github.com/matthiasgruenwald/moo-gpt/blob/a53f5acf43cedf04d46edf916bbff0545bac89d1/auth-middleware.js
[cp-adr]: https://github.com/matthiasgruenwald/moodle-coursepilot/blob/467c954508291a700215b6cb4c2940ac85d29db4/docs/adr/0027-versionslinien-branches-und-moodle-mindestversion.md
[cp-ci]: https://github.com/matthiasgruenwald/moodle-coursepilot/blob/467c954508291a700215b6cb4c2940ac85d29db4/.github/workflows/native-ci.yml
[cp-package]: https://github.com/matthiasgruenwald/moodle-coursepilot/blob/467c954508291a700215b6cb4c2940ac85d29db4/package.json
[cp-mirror-build]: https://github.com/matthiasgruenwald/moodle-coursepilot/blob/467c954508291a700215b6cb4c2940ac85d29db4/scripts/build-mirror-export.js
[cp-mirror-ci]: https://github.com/matthiasgruenwald/moodle-coursepilot/blob/467c954508291a700215b6cb4c2940ac85d29db4/.github/workflows/mirror-sync.yml
[cp-artifact]: https://github.com/matthiasgruenwald/moodle-coursepilot/blob/467c954508291a700215b6cb4c2940ac85d29db4/scripts/ci/verify-native-artifact.php
[cp-testing]: https://github.com/matthiasgruenwald/moodle-coursepilot/blob/467c954508291a700215b6cb4c2940ac85d29db4/docs/agents/testing.md
[cp-js-runner]: https://github.com/matthiasgruenwald/moodle-coursepilot/blob/467c954508291a700215b6cb4c2940ac85d29db4/scripts/ci/run-js-tests.js
[cp-run]: https://github.com/matthiasgruenwald/moodle-coursepilot/actions/runs/36885192594
[cp-coverage-job]: https://github.com/matthiasgruenwald/moodle-coursepilot/actions/runs/36885192594/job/110446882744
[cp-js-job]: https://github.com/matthiasgruenwald/moodle-coursepilot/actions/runs/36885192594/job/110446883216
[cp-oauth-test]: https://github.com/matthiasgruenwald/moodle-coursepilot/blob/467c954508291a700215b6cb4c2940ac85d29db4/Plugin/src/local_coursepilot/tests/oauth_lib_test.php
[cp-dispatcher-test]: https://github.com/matthiasgruenwald/moodle-coursepilot/blob/467c954508291a700215b6cb4c2940ac85d29db4/Plugin/src/local_coursepilot/tests/dispatcher_test.php
[cp-remote-test]: https://github.com/matthiasgruenwald/moodle-coursepilot/blob/467c954508291a700215b6cb4c2940ac85d29db4/Plugin/src/local_coursepilot/tests/remote_access_test.php
[cp-webdav-test]: https://github.com/matthiasgruenwald/moodle-coursepilot/blob/467c954508291a700215b6cb4c2940ac85d29db4/Plugin/src/local_coursepilot/tests/webdav/curl_transport_test.php
[cp-ticket-test]: https://github.com/matthiasgruenwald/moodle-coursepilot/blob/467c954508291a700215b6cb4c2940ac85d29db4/Plugin/src/local_coursepilot/tests/werkbank_ticket_test.php
[cp-privacy-test]: https://github.com/matthiasgruenwald/moodle-coursepilot/blob/467c954508291a700215b6cb4c2940ac85d29db4/Plugin/src/local_coursepilot/tests/privacy/provider_test.php
[cp-context-test]: https://github.com/matthiasgruenwald/moodle-coursepilot/blob/467c954508291a700215b6cb4c2940ac85d29db4/Plugin/src/local_coursepilot/tests/context_files_test.php
[cp-write-test]: https://github.com/matthiasgruenwald/moodle-coursepilot/blob/467c954508291a700215b6cb4c2940ac85d29db4/Plugin/src/local_coursepilot/tests/write_gate_test.php
[cp-import-test]: https://github.com/matthiasgruenwald/moodle-coursepilot/blob/467c954508291a700215b6cb4c2940ac85d29db4/Plugin/src/local_coursepilot/tests/external/import_questions_xml_test.php
[cp-dp]: https://github.com/matthiasgruenwald/moodle-coursepilot/blob/467c954508291a700215b6cb4c2940ac85d29db4/test/data-protection-contract.test.js
[cp-local-path-test]: https://github.com/matthiasgruenwald/moodle-coursepilot/blob/467c954508291a700215b6cb4c2940ac85d29db4/test/local-context-paths.test.js
