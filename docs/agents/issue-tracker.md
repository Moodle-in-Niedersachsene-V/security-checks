# Issue tracker: GitHub

Issues und Spezifikationen liegen in GitHub Issues dieses
Repositories. Für Operationen die gh-CLI verwenden;
das Repository aus dem Git-Remote bestimmen.

- Lesen: `gh issue view <nummer> --comments`
- Auflisten: `gh issue list` mit passenden Status- und Label-Filtern
- Erstellen: `gh issue create --title "..." --body-file <datei>`
- Kommentieren: `gh issue comment <nummer> --body-file <datei>`
- Labels: `gh issue edit <nummer> --add-label "..." --remove-label "..."`
- Schließen: `gh issue close <nummer> --comment "..."`

Mehrzeilige Inhalte über eine Datei mit echten Zeilenumbrüchen übergeben.
„Im Issue-Tracker veröffentlichen“ bedeutet: GitHub Issue erstellen.
„Relevantes Ticket laden“ bedeutet: Issue einschließlich Kommentaren lesen.

## Pull requests as a triage surface

**PRs as a request surface: no.**
