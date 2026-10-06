# Planung: Welches Vereinsplugin eignet sich als Pilot?

Stand: **06.10.2026**. Reine Planung auf Wunsch des Nutzers: Hauptbranch-Code und Dokumentation gelesen und Dateiumfang gezählt. Für diese Planung keine neuen Testbranches, Tests, Laufumgebungen oder Änderungen an den Plugins angelegt. Der vorhandene Easy-Admin-Pilot bleibt separat liegen; seine Übernahme ist nicht Gegenstand dieses Schritts.

## Umfang aller 14 öffentlichen Vereinsplugins

Physische Zeilen in PHP-/JS-Quelldateien, einschließlich Kommentaren, Leerzeilen und eingebettetem Code. Sprachdateien, Tests, typische Dependency-Verzeichnisse und generierte AMD-Builds/minifizierte JS-Dateien ausgeschlossen. Keine bereinigten SLOC und keine Qualitätsbewertung. Templates/CSS sind zusätzlich in der [maschinenlesbaren Erhebung](research/verein-groessenplanung.json) erfasst. JS = 0 bedeutet keine separat gezählte JS-Quelldatei; JavaScript kann in PHP eingebettet sein. Beim MRBS-Plugin zählt die eingebundene Webanwendung mit. Das externe Python-Dashboard ist kein Moodle-Plugin und steht außerhalb dieses Größenvergleichs.

Die Einschätzung der Pilot-Eignung ist unsere Abwägung anhand gelesener Funktionsbeschreibungen und ausgewählter Einstiegspunkte; keine vollständige Sicherheitsprüfung jedes Projekts. Die Links fixieren die gelesenen Hauptbranch-Stände.

| Plugin | PHP-Zeilen | JS-Zeilen | Zweck | Eignung als erster Pilot |
| --- | ---: | ---: | --- | --- |
| [local_plugininfo](https://github.com/Moodle-in-Niedersachsene-V/local_plugininfo/tree/c785a842c6c280e9a95d12f43a8d6d1eafef97dd) | 348 | 0 | Admin-Webservices für Pluginliste und Nutzungszahlen | Sehr einfacher Rechtepilot; deckt wenig Schreib-/Dateiverhalten ab. |
| [tiny_textgestaltung](https://github.com/Moodle-in-Niedersachsene-V/tiny_textgestaltung/tree/7ade60983a2401b84ccfa42d0ecea865b5b00484) | 475 | 385 | Schriften, Größen und Farben im Tiny-Editor | Kleiner Editor-/Ausgabepilot; weniger geeignet als erster umfassender Sicherheitsablauf. |
| [local_peertubeupload](https://github.com/Moodle-in-Niedersachsene-V/local_peertubeupload/tree/ff10053f8299ce08f0602d42c3427b75600847d9) | 1.183 | 0 | Video-Upload nach PeerTube, persönlicher Kanal | Interessant für Upload/Rechte; OAuth-Repository und externe Schnittstelle erhöhen den Aufbauaufwand. |
| [repository_nextclouddirect](https://github.com/Moodle-in-Niedersachsene-V/repository_nextclouddirect/tree/2fd672b6e29b4d68a670bc271bdbdae78429f751) | 1.266 | 0 | Nextcloud-Upload aus dem Moodle-Dateipicker | Klein, aber OAuth/WebDAV und abhängige Repository-Integration; eher zweiter Integrationspilot. |
| [local_nextcloudbackup](https://github.com/Moodle-in-Niedersachsene-V/local_nextcloudbackup/tree/63a0ca9589e03161e1d587cdb677f5c42b40dccb) | 1.531 | 0 | Kursbackup mit Übertragung und Aufbewahrung in Nextcloud | Interessant für Datenverlust/Backupinhalt; hängt am Nextcloud-Transport, daher größerer Testumfang als die Zeilenzahl suggeriert. |
| [block_kursfilter](https://github.com/Moodle-in-Niedersachsene-V/block_kursfilter/tree/ad60a3222fededea6aaad01724266aea728e49ba) | 1.909 | 450 | Kurssuche, Bewertungen, öffentliche Backups und Gast-Pool | Empfehlung für einen überschaubaren, aussagekräftigen Pilot; Gast-/Freigaberegeln zuerst klären. |
| [repository_peertubeoauth](https://github.com/Moodle-in-Niedersachsene-V/repository_peertubeoauth/tree/20f2dff51bba3fc67414c275df02fc01eb3d065a) | 1.984 | 289 | PeerTube-Dateipicker, gemeinsame Konten und persönliche/Gruppenkanäle | Wichtige Rechte-/Token-/Kanalgrenzen; externe Abhängigkeiten und gemeinsame Konten machen den Einstieg aufwendiger. |
| [block_questionfilter](https://github.com/Moodle-in-Niedersachsene-V/block_questionfilter/tree/a7bcf379868cda395b85ab6176f97009f492ed9f) | 2.313 | 987 | Kursübergreifende Fragebanksuche und Export | Sehr guter Kandidat für Rechte/Fremdobjekte/Export; Fragebank-Kontexte und Versionsunterschiede berücksichtigen. |
| [mod_videolearn](https://github.com/Moodle-in-Niedersachsene-V/mod_videolearn/tree/c178113909d8abe09c127cb6c5dc114aa10272a6) | 3.680 | 1.964 | Video-Lernaktivität mit Player, Lern-/Abschlussfunktionen | Breiterer Pilot: Datei-/Kurs-/Nutzergrenzen plus umfangreicher Browserteil; später sinnvoll. |
| [local_calendarcategories](https://github.com/Moodle-in-Niedersachsene-V/local_calendarcategories/tree/12879fd026de328f08f82ea2796f9b4678a3797a) | 3.780 | 0 | Kalendergruppen mit Mitgliedern und zugeordneten Terminen | Sehr guter alternativer Pilot für Rechte, Mitgliedschaften, Schreibaktionen und Privacy; keine notwendige externe Serverintegration. |
| [local_emailclient](https://github.com/Moodle-in-Niedersachsene-V/local_emailclient/tree/4f34510313b760c7fa6c175995d8f5da0a00b75f) | 4.973 | 249 | IMAP-/SMTP-Mailclient in Moodle | Hoher Sicherheitsnutzen, aber Netzwerk, Kontogeheimnisse, Nachrichten und Anhänge; als erster Pilot zu breit. |
| [local_kursassistent](https://github.com/Moodle-in-Niedersachsene-V/local_kursassistent/tree/c66920bac2a66623311c8648da93fe49c4564b99) | 6.553 | 4.429 | Kursgestaltung, Vorlagen, Materialien, Lernpfade und Statistik | Großer Funktions- und Browserumfang, mehrere optionale Integrationen; später gezielte Teilpiloten. |
| [local_easyadmin](https://github.com/Moodle-in-Niedersachsene-V/local_easyadmin/tree/492cabe8eeb23d755cb16c2e417f74bbdfe790be) | 7.114 | 0 | Schul-Admin-Dashboard, Konten, Kurse und Einstellungen | Sicherheitsrelevant, aber breit. Vorhandenen Teilpilot liegen lassen; kein aktueller Mergeauftrag. |
| [block_mrbs_nds](https://github.com/Moodle-in-Niedersachsene-V/block_mrbs_nds/tree/5176bb5fdbef96eee4fbc332725b496564d36990) | 9.369 | 920 | Raum-/Ressourcenbuchung mit eingebundener MRBS-Weboberfläche | Größter PHP-Umfang im Vereinsvergleich; Buchungs-/Konflikt-/Rechte- und Legacy-Webpfade machen den Einstieg aufwendiger. |

## Empfehlung: Kursfilter, alternativ Kalenderkategorien

**Der Kursfilter ist ausreichend anspruchsvoll.** Sein PHP-Umfang ist klein, seine Schutzgrenzen sind vielfältig: Suchergebnisse, begrenzte Anfragemengen, öffentliche Schreibaktionen, herunterladbare Kurssicherungen und Anmeldung über Pool-Konten. Viele sinnvolle Fälle lassen sich innerhalb Moodles aufbauen; ein externer OAuth-/Mail-/Dateiserver ist für diesen Einstieg nicht erforderlich.

Die wichtige Einschränkung: Wer ausschließlich die Suchfunktion testet, erprobt nur einen Teil des Sicherheitsstandards. Ein aussagekräftiger Pilot sollte die wesentlichen unterschiedlichen Grenzen mit aufnehmen und offene Teile ausdrücklich benennen. Testanzahl oder Zeilenzahl allein entscheidet das nicht.

**Kalenderkategorien** erweitert den Kalender um Kategorien für etwa Fachschaften, Schulbereiche oder Gremien. Nutzer werden als Mitglieder zugeordnet; vorhandene Moodle-Termine werden mit Kategorien verknüpft. Zusätzlich gibt es Beitreten/Austreten, Verwaltungsrechte und eigene Daten-/Privacy-Pfade. Das ist kein reiner Farbfilter. Die Mitgliedschafts- und Rechteprüfung in eigenen Manager-Klassen eignet sich gut für überschaubare Tests. Quellen: [Beschreibung](https://github.com/Moodle-in-Niedersachsene-V/local_calendarcategories/blob/12879fd026de328f08f82ea2796f9b4678a3797a/README.md), [Kategorie-/Mitgliedschaftslogik](https://github.com/Moodle-in-Niedersachsene-V/local_calendarcategories/blob/12879fd026de328f08f82ea2796f9b4678a3797a/classes/category_manager.php).

Wenn der erste Pilot besonders einfach aufzubauen sein soll, ist Kalenderkategorien trotz höherer Zeilenzahl eine gute Alternative: weniger spezielle Gast-/Sessionabläufe, klar abgegrenzte Rechte- und Mitgliedschaftsfunktionen. Kursfilter ist die Empfehlung, wenn wir bewusst auch öffentliche Funktionen, Dateien und Sessions im Verfahren erproben wollen.

## Geplanter Prüfungsumfang für den Kursfilter

Die folgenden Fälle sind Planvorschläge, keine ausgeführten oder bestandenen Tests.

| Grenze | Was ein sinnvoller Pilot nachweisen sollte |
| --- | --- |
| Suche und Sichtbarkeit | Zulässige Kurse auffindbar; versteckte, ausgeschlossene oder fachlich nicht freigegebene Kurse nicht sichtbar. Kategorie-/Kontextregeln und Site-Kurs berücksichtigen; manipulierte Suchparameter verändern den Umfang nicht unberechtigt. |
| Ergebnis-/Anfragelimits | Serverseitige Grenzen greifen auch bei manipulierten Limits; zulässige Suche funktioniert weiterhin. Gemeinsame Gastidentität und Rate-Limit-Verhalten fachlich prüfen. |
| Bewertungen | Nur erlaubte HTTP-Methode und gültige Sesskey führen zur Speicherung; Werte außerhalb 1–5 und unzulässige Kurs-IDs verändern keine Daten. Wiederholte Bewertung desselben Cookies entsprechend der vorgesehenen Regel behandeln. |
| Backup-Download | Zulässiger öffentlicher Download funktioniert; versteckter/falscher/Site-Kurs und fehlende Datei werden abgelehnt. Fremde Kurs-ID darf keinen unerlaubten Backup-Zugriff erzeugen. |
| Backup-Inhalt | Tatsächlich erzeugtes Archiv auf die vereinbarten auszuschließenden Nutzer-/Teilnahmedaten prüfen. Abgeschaltete Backup-Schalter allein beweisen nicht, dass alle Inhalte für öffentliche Verteilung geeignet sind. |
| Gast-Pool | Gewünschter Gastzugang funktioniert; verbotene Kurs-/Nutzer-/Aktionsgrenzen bleiben geschützt. Die zugewiesene Rolle und ihre tatsächlichen Rechte prüfen, außerdem Wiederverwendung, aktive Belegung, abgelaufene Reservierung und Sessionwechsel. |
| Ausgabe | Manipulierte Kurs-/Tag-Inhalte bleiben bei eigener Aufbereitung/Browserausgabe ungefährlich; vorhandene Moodle-Ausgabemechanismen korrekt eingebunden. |

Direkte Quellen: [Suche/Limit](https://github.com/Moodle-in-Niedersachsene-V/block_kursfilter/blob/ad60a3222fededea6aaad01724266aea728e49ba/classes/external.php), [Bewertungsrequest](https://github.com/Moodle-in-Niedersachsene-V/block_kursfilter/blob/ad60a3222fededea6aaad01724266aea728e49ba/rate.php), [Download](https://github.com/Moodle-in-Niedersachsene-V/block_kursfilter/blob/ad60a3222fededea6aaad01724266aea728e49ba/backup.php), [Backup-Erzeugung](https://github.com/Moodle-in-Niedersachsene-V/block_kursfilter/blob/ad60a3222fededea6aaad01724266aea728e49ba/classes/backup_helper.php), [Gastanmeldung](https://github.com/Moodle-in-Niedersachsene-V/block_kursfilter/blob/ad60a3222fededea6aaad01724266aea728e49ba/guest_login.php), [Pool-Verwaltung](https://github.com/Moodle-in-Niedersachsene-V/block_kursfilter/blob/ad60a3222fededea6aaad01724266aea728e49ba/classes/pool_manager.php).

**Vor der Fallfestlegung ist eine fachliche Entscheidung wichtig:** Der Code behandelt sichtbare Kurse als für öffentliche Downloads beziehungsweise Pool-Zugang geeignet. Die Pool-Konten erhalten die Rolle `teacher` (Trainer ohne Bearbeitungsrecht). Zu klären ist, welche Kurse wirklich öffentliche Demo-/Materialkurse sein sollen und welche Daten/Aktionen die Rolle dort erreichen darf. Sichtbarkeit eines Kurses, Download-Freigabe und Gastberechtigung müssen im beabsichtigten Betrieb zusammenpassen. Das ist eine Planungsfrage und ein wichtiger Prüfgegenstand; hier wird daraus keine reproduzierte Sicherheitslücke behauptet.

## Sinnvolle Reihenfolge für eine spätere Umsetzung

1. Gewünschte öffentliche Nutzung, Kursfreigaben und Gastrechte festhalten. Daraus zulässige/unzulässige Fälle ableiten.
2. Kleinen Fallplan für Suche, Limits, Bewertungen und Dateien festlegen; Gast-Pool und Archivinhalt mit eigenen Nachweisen einplanen.
3. Testarten zuordnen: PHPUnit für produktive Manager-/External-Funktionen, echte Request-/Integrationstests für Sesskey-/HTTP-/Sessiongrenzen und Dateitransfer. Diese Endpunktgrenzen nicht durch Tests nachgebauter Logik ersetzen.
4. Erst danach einen separaten Branch erstellen und Tests implementieren. Aufwand und Nachweiswert dokumentieren; gezielte Gegenproben nach Umsetzung vorsehen.
5. Den erprobten Ablauf anschließend auf Fragefilter oder Kalenderkategorien übertragen. Externe Integrationen bilden einen späteren eigenen Pilot.

Ein Pilot am Kursfilter kann einen brauchbaren Ablauf für ein kleines Plugin liefern. Er ersetzt keine spätere Prüfung der OAuth-/Netzwerkgrenzen von Nextcloud-/PeerTube-/Mail-Plugins und keine Wartungs-/Betriebsfreigabe. Aktuell ist keine Umsetzung aus dieser Planung begonnen.
