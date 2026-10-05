<h1 align="center">Athlete Atlas</h1>

<p align="center">
  <b>The paperwork side of a sports team, in one place.</b><br />
  Rosters, medical visas and match results for a club's coach.
</p>

I played performance handball for years, won two national titles, and captained my team. The matches were the easy part to keep track of. The paperwork wasn't: every player needs a registration card and a valid medical visa, and somebody has to notice when one expires before the referee does.

Athlete Atlas keeps that in one place. A coach creates a team, adds players by hand or from an Excel sheet, and logs every match with the score and the official report. Each player's medical visa date sits next to their name. The app doesn't warn you when one expires yet, so you still have to read the dates.

## What it keeps track of

- **Players.** Name, father's initial, birth date, CNP, registration number, university, and the date their medical visa runs out. Each player is marked active or inactive.
- **Matches.** Opponent, date, score, and the match report as a PDF.
- **Results.** Wins, draws and losses for each team, drawn as a chart.
- **Teams.** Type the sport in Romanian: `fotbal`, `handbal`, `baschet`, `volei` or `tenis`. Score fields only show up for these five. The dashboard shows each coach only the teams they created.

The interface is in Romanian, like the clubs it was made for.

## Importing players from Excel

The team page imports players from the first sheet of an `.xlsx` or `.xls` file. The headers have to match these exactly, diacritics included, and every column has to be formatted as Text, because the import reads each cell as text:

| Nume | Prenume | Inițiala tatălui | Data nașterii | CNP | Număr legitimație | Universitate | Expirare viză medicală | Activ |
|---|---|---|---|---|---|---|---|---|
| Popescu | Ion | M | 2003-04-12 | ... | 1234 | UPT | 2026-09-30 | da |

## Under the hood

React 19 and Material UI in the browser. Firebase does the rest: Authentication for sign-in (email with verification, or Google), Firestore for teams, players and matches, and Storage for the PDF reports. Recharts draws the results chart and SheetJS reads the Excel files.

Data lives in one `teams` collection, with each team's players and matches nested under it:

```text
teams/{teamId}
teams/{teamId}/members/{memberId}
teams/{teamId}/matches/{matchId}
```

## Run your own

You need Node.js and a Firebase project with Authentication (email/password and Google), Firestore and Storage turned on.

1. Put your project's config in `src/firebase.jsx`.
2. Deploy the rules in this repo, which limit every team to the user who created it: `firebase use --add`, then `firebase deploy --only firestore:rules,storage`.
3. Install and start:

```bash
npm install
npm start
```

`npm run build` produces the production build.

This is a portfolio project. It stores CNPs, which count as personal data under GDPR, so review the rules and your obligations before you put real players in it.

## Who built it

[Luka Stoicov](https://github.com/LukaStoicov) and I built it in January 2025. I did sign-in, email verification, Google login, error handling and the theme. Luka built the database layer, the dashboard and the team page, and came back in October 2025 to add the Excel import and the results chart.

[Andrei Stolojan](https://github.com/AndreiStolojan) · [LinkedIn](https://www.linkedin.com/in/andrei-stolojan/)
