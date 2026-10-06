# Vorschlag für Plugin-Betrieb und Wartbarkeit beim NLQ

Stand: **05.10.2026**. Diskussionsvorschlag des Repositorys, keine bereits beschlossene NLQ-Richtlinie. Ausgangslage laut Gespräch: Das NLQ möchte seinen gewachsenen Plugin-Bestand systematisch betreuen und Sicherheit sowie langfristige Wartbarkeit bewerten. Ein vollständiger Bestand der tatsächlich eingesetzten NLQ-Plugins liegt hier noch nicht vor; die bisherige Vereins-/ByCS-Inventur ersetzt ihn nicht.

## Ziel und Umfang

Für jedes eingesetzte Plugin soll nachvollziehbar sein: Ist die konkrete Version technisch ausreichend geprüft, wer hält sie einsatzfähig, bis wann ist die Betreuung zugesagt und wie lässt sie sich bei Bedarf übernehmen oder ablösen?

Vorgeschlagen wird ein gemeinsames Verfahren für bestehende Plugins und Neuaufnahmen. Bestehende Plugins zunächst inventarisieren und nach Risiko prüfen: öffentliche Schnittstellen, erhöhte Rechte, externe Daten-/Dateiverbindungen und eigene Tokenlogik zuerst. Auch externe Tools und betriebsnotwendige Plugin-Abhängigkeiten gehören mit ihren eigenen Verantwortlichkeiten in den Nachweis.

Der [technische Prüfstandard](pruefstandard-entwurf.md) liefert anwendbare Sicherheitsfälle. Dieses Dokument ergänzt die Wartungs- und Betriebsentscheidung. Ein Prozentwert zeigt Prüfplan-Fortschritt; die Entscheidung folgt den konkreten Befunden und der tragfähigen Betreuung.

## Schlanke Mindestnachweise zur Wartbarkeit

Die Kriterien sind unser Vorschlag. Pro Kriterium genügen zunächst ein dokumentierter Nachweis oder eine konkrete offene Maßnahme; kein zusätzlicher Scanner ist nötig.

| ID | Kriterium | Ausreichender Ausgangsnachweis |
| --- | --- | --- |
| W1 | Verantwortliche Betreuung und Ausfallplan | Benannte betreuende Stelle/Person, erreichbarer Kontakt und benannte Übernahmeoption bei Ausfall; Verein, Dienstleister oder NLQ mit zugesagter Kapazität. Eine theoretisch verfügbare Community genügt als Übernahmeplan nicht. |
| W2 | Quellcode und Übernahmemöglichkeit | Quellcode, Lizenzbedingungen und erforderliche Build-/Release-Unterlagen verfügbar; berechtigte Stelle kann Fixes und Releases bereitstellen. Notwendige Repository-/Paket-/Dienstzugänge lassen sich kontrolliert übergeben; keine Veröffentlichung von Secrets. |
| W3 | Reproduzierbare Pflege | Kurze Anleitung zu Installation, Tests, Build/Release und eigenen Daten-/Abhängigkeiten. Eine zweite befähigte Person oder übernehmende Stelle kann anhand dieser Anleitung einen Testlauf und ein Testpaket erzeugen. Ergebnis der Übergabeprobe festhalten. |
| W4 | Supportzeitraum und Upgradepfad | Eingesetzte sowie zugesagte Moodle-/PHP-/DB-Versionen, zugesagtes Betreuungsende und nächster Upgrade-/Prüftermin dokumentiert. Vor Plattformwechsel Kompatibilität belegen oder Ablösung planen. |
| W5 | Umgang mit Sicherheitsmeldungen | Meldeweg, verantwortliche Triage und abgestimmte Reaktions-/Behebungsfristen nach Risiko; Möglichkeit zur kurzfristigen Einschränkung/Deaktivierung. Konkret vereinbarte Fristen festhalten statt allein aus letzter Commit-Aktivität Wartbarkeit abzuleiten. |
| W6 | Wiederholbare Prüfungen | Die vereinbarten technischen Fälle sind ausführbar, ihre Ergebnisse der verwendeten Version zugeordnet und relevante Befunde nachverfolgbar. Eigene kritische Schutzlogik möglichst automatisiert testen. |
| W7 | Geordneter Ausstieg | Abhängige Funktionen und Daten bekannt; Export/Migration, Sicherung und notwendige Schritte zur sicheren Deaktivierung/Deinstallation beschrieben. Bei Änderungen oder Entfernung Datenverlust und Auswirkungen auf andere Plugins prüfen. |

Ein Einzelentwickler kann diese Nachweise mit einer vereinbarten Übernahme durch Verein oder Dienstleister erfüllen. Entscheidend ist, ob bei Ausfall tatsächlich jemand mit Zugriff, Wissen und Kapazität übernehmen kann. Auch mehrere Entwickler ohne Übergabe- und Supportregelung lösen diese Frage nicht.

**Angebotsdauer:** Im Steckbrief ein konkretes Datum für die zugesagte Betreuung nennen. Weiterbetrieb vor dessen Ablauf anhand von Sicherheitsstand, Plattform-Support und Betreuung neu entscheiden. Kein pauschales Lebensalter für Plugins festlegen und aus erfolgreicher heutiger Prüfung keine unbegrenzte Wartungszusage ableiten. Moodle veröffentlicht seine Plattform-Supportzeiträume in der [Releaseübersicht](https://moodledev.io/general/releases); sie ersetzen keine eigene Plugin-Wartungszusage.

Der [ergänzende Qualitätsablauf](qualitaetsablauf-entwurf.md) konkretisiert W3/W6 für die tägliche Entwicklung: reproduzierbare gemeinsame Prüfkommandos, klare Testzuordnung und Verbesserungen an geänderter Logik. Seine Einführung kann im Pilotprojekt erprobt werden; die Kandidaten sind noch keine zusätzliche NLQ-Aufnahmebedingung.

## Vorgeschlagene Betriebsentscheidungen

| Ergebnis | Voraussetzung / Umgang |
| --- | --- |
| Weiterbetrieb / Aufnahme empfohlen | Anwendbare technische Pflichtfälle belegt, keine ungeklärte freigabeblockierende Sicherheitslücke; Wartungsnachweise und erforderliche institutionelle Freigaben vorhanden. Nächsten Prüftermin festlegen. |
| Nachbesserung erforderlich | Behebbarer Befund oder fehlender Nachweis: konkrete Maßnahme, zuständige Stelle und Frist festlegen. Ein befristeter Weiterbetrieb braucht eine ausdrücklich begründete Risikoentscheidung und gegebenenfalls Einschränkungen. |
| Betrieb aussetzen / Ablösung vorbereiten | Ausnutzbare erhebliche Lücke ohne wirksame Absicherung, ausdrückliches Nutzungsverbot oder nicht rechtzeitig wiederherstellbare Betreuung/Kompatibilität: betroffene Nutzung einschränken oder deaktivieren; sicheren Ersatz/Migration planen. |
| Prüfung noch offen | Nachweise noch nicht untersucht: Status und Termin sichtbar machen. Das ist keine erfolgreiche Prüfung und kein bewiesener technischer Fehler. Entscheidung über vorläufigen Betrieb gesondert festhalten. |

Ein kleiner nachweisbarer Dokumentationsmangel und eine ausnutzbare Zugriffslücke brauchen unterschiedliche Maßnahmen. Eine bekannte freigabeblockierende Lücke lässt sich nicht mit guter Code-Coverage oder guter Wartbarkeit ausgleichen. Bei ausbleibender Nachbesserung erneut entscheiden: beheben/übernehmen lassen oder Nutzung beenden.

Das NLQ beziehungsweise die zuständige Betriebsstelle entscheidet über Aufnahme, Ausnahmen und Weiterbetrieb; der Verein liefert Prüfung, Nachweise und gegebenenfalls Wartung. Vereinbarte technische Änderungen können durch den ursprünglichen Maintainer oder eine berechtigte übernehmende Stelle erfolgen. Die hier genannten Kriterien begründen noch keine automatische Entfernung bestehender Plugins.

## Datenschutz und institutionelle Zulässigkeit

Technische Schutzprüfung, Wartbarkeit und Datenschutz-/Nutzungsfreigabe getrennt dokumentieren. Ein technisch bestandenes Plugin kann zusätzliche organisatorische oder institutionelle Bedingungen haben. Eine geltende Untersagung bleibt maßgeblich; ein Scan hebt sie nicht auf. Umgekehrt begründet ein Datenschutzbefund allein keine Aussage über die Codequalität oder Ausnutzbarkeit.

Das im Gespräch genannte Microsoft-365-Beispiel wird hier als geschilderter Hintergrund eingeordnet. Für eine Bewertung konkreter Produkte wären der tatsächliche Bescheid, sein Geltungsbereich und die betroffene Version erforderlich; dazu wird hier keine eigene Tatsachenbehauptung getroffen.

## Wiederholungsprüfung ohne unnötigen Aufwand

Unser Vorschlag: jährlich eine kurze Prüfung des Bestands, der Betreuung und der Supporttermine. Technische Fälle bei relevanten Plugin-Änderungen, vor Plattform-Upgrades sowie bei Sicherheitsmeldungen erneut prüfen; anlasslose vollständige Wiederholung jeder Prüfung ist damit nicht verlangt. Die Intervalle und Reaktionsfristen sind mit dem NLQ festzulegen.

Bei neuen Endpunkten, Rechten, Datenflüssen oder Abhängigkeiten den Fallplan aktualisieren. Bei Wechsel der betreuenden Stelle eine Übergabeprobe durchführen. Vor Ende einer Wartungszusage rechtzeitig Verlängerung, Übernahme oder Migration entscheiden.

## Ein gemeinsamer Steckbrief pro Plugin

```markdown
Plugin / Zweck / verantwortliche Betriebsstelle:
Eingesetzte Version / Commit / Paket / betroffene Instanzen:
Abhängige Plugins, externe Dienste und eigener Datenbestand:

Technische Prüfung: Planversion / Ergebnis / Beleglinks / offene Befunde:
Wartbarkeit W1–W7: Nachweise / offene Maßnahmen:
Betreuende Stelle / Kontakt / vereinbarte Übernahmeoption:
Zugesagte Betreuung bis / unterstützte Plattformen:
Meldeweg / vereinbarte Reaktions- und Behebungsfristen:
Datenschutz-/Nutzungsentscheidung: zuständige Stelle / Quelle / Bedingungen:

Betriebsentscheidung / entscheidende Person oder Stelle / Datum:
Maßnahmen / Zuständigkeit / Frist / Einschränkungen:
Nächste Prüfung / Upgrade- oder Ablösetermin:
```

## Was noch mit dem NLQ festzulegen ist

- Vollständiger Plugin-Bestand mit eingesetzten Versionen und Verantwortlichkeiten.
- Verbindliche Mindestnachweise, Befundklassen und Entscheidungszuständigkeiten.
- Betreuungszusagen, Übernahmekapazität, Prüfintervalle und Fristen.
- Umgang mit befristeten Ausnahmen und sicherer Migration bei Ablösung.
