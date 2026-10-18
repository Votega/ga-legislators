# ga-legislators

Data for all members of Georgia's General Assembly, maintained by [VoteGA.org](https://votega.org). The **current roster** refreshes daily; **voting records** refresh weekly. A General Assembly is a two-year *biennium* — a regular session plus any special sessions the Governor convenes — and the upstream data is replaced wholesale over time, so this repo keeps a permanent **per-session archive**: each session's votes (and the frozen end-of-biennium roster) live in their own folder and are never lost.

> **Looking for Georgia's U.S. Senators and Representatives?** They are in the companion repo [`ga-federal-legislators`](https://github.com/Votega/ga-federal-legislators). It is separate because it comes from a different source (Congress.gov and the House Clerk / Senate roll-call XML) and identifies members by Bioguide ID rather than the OCD person ID used here, so the two do not join.

## Current data

The live roster — always the members serving right now.

| File | Format | Contents | Refresh |
|---|---|---|---|
| `data/all.json` | JSON | All members (House + Senate combined) | Daily |
| `data/house.json` | JSON | House of Representatives members only | Daily |
| `data/senate.json` | JSON | Senate members only | Daily |
| `data/members.csv` | CSV | **Spreadsheets** — one row per legislator (executives excluded) | Daily |
| `data/members.schema.json` | JSON Schema | Validating / typing the member data | Daily |
| [`ROSTER.md`](ROSTER.md) | Markdown | **Reading** — the current Senate & House roster | Daily |

> **Note:** `all.json` also contains 4 statewide **executives** (`chamber: "executive"`) — filter to `chamber` in `["Senate", "House of Representatives"]` for legislators only. `members.csv` and `ROSTER.md` already exclude them.

## Per-session archive

Each session in the biennium gets its own folder under `sessions/<slug>/`, **never overwritten** when another session begins. Slugs: the **regular** session collapses to its biennium span (`2025-2026`); a **special** session keeps its identifier (`2026-ss`). Roll-call votes are archived per session; the roster is frozen once per General Assembly (see below).

| File | Format | Contents |
|---|---|---|
| [`latest.json`](latest.json) | JSON | **Start here** — the biennium, `currentSession`, and a `sessions[]` list with each session's file paths |
| `sessions/<slug>/votes.json` | JSON | That session's passage-vote roll calls + how each member voted (`memberVotes` keyed by member `id`) |
| `sessions/<slug>/votes.csv` | CSV | Spreadsheets — one row per roll-call vote |
| `sessions/<slug>/votes.schema.json` | JSON Schema | Validating / typing the vote data |
| `sessions/<biennium>/members.json` | JSON | The roster as it stood at the end of the General Assembly (frozen at sine die) |
| `sessions/<biennium>/members.csv` | CSV | Spreadsheet view of the frozen roster |
| `sessions/<biennium>/ROSTER.md` | Markdown | Readable snapshot of who served that General Assembly |

Sessions in the current biennium: **2025-2026** (regular) and **2026-ss** (2026 special session). To resolve them programmatically, read `latest.json` and iterate its `sessions[]` (or follow `currentSession`) rather than hard-coding a slug. There is no per-member-vote CSV here (≈235 members × ~2,200 votes is too large for a repo file); use each session's `votes.json` `memberVotes`, joined to `members.csv` / `votes.csv`.

> The **roster** is frozen once per General Assembly, at `sessions/<biennium>/members.*` — the membership is the same across a biennium's regular and special sessions. It appears only after the deliberate end-of-biennium freeze; until then the current roster lives at `data/all.json`. `latest.json`'s `rosterArchive` names its path.

### Voting scorecard (derived)

`sessions/2025-2026/scorecard.csv` · `scorecard.json` — party-unity and
participation scores for every sitting member, computed from `votes.json`
joined with the roster. Party unity = share of party-line roll calls (own-party
majority opposite the other party's majority) voted with your own caucus;
participation = share of your chamber's passage roll calls on which you cast a
Yea or Nay. See `scorecard.schema.json`; root pointer at `scorecard-latest.json`.

## Member records

Field definitions live in `data/members.schema.json` (and the same file under each `sessions/<biennium>/`); this section covers what a schema can't say.

- **`legisGaGovId` is the stable identifier.** It is the General Assembly's own id for the member, and it is what links to the member's page on legis.ga.gov.
- **`id` is the join key into votes, but it is assigned once and never changes.** It is normally an Open States person id (`ocd-person/…`), carried as a cross-reference; a member Open States has not catalogued is keyed `legis-<legisGaGovId>`. Prefer `legisGaGovId` when you need an identity that cannot move.
- **Not every record is a legislator.** `data/all.json` also holds the statewide executives (`chamber: "executive"`). Filter to `chamber` in `["Senate", "House of Representatives"]` for legislators only; `members.csv` and `ROSTER.md` already exclude them.
- **`status` is `null` for a sitting member.** Otherwise it is one of `"Resigned"`, `"Removed"`, `"Suspended"`, `"Deceased"` or `"Vacant"`, with `statusDate` and a `statusNote` (which may contain a markdown link to the source). These fields are editorial: the General Assembly's own service records who holds a seat, not who has left it, so VoteGA maintains them by hand.
- **Optional fields are `null`, not empty strings,** when unknown.
- Records carry more than the schema lists (for example committee roles, occupation and residence); the schema allows additional properties.

## Voting records (`sessions/<slug>/votes.json`)

Passage votes — final up-or-down floor votes on a bill — for one session of the General Assembly, with how every member voted on each one. Procedural motions, amendments, and committee votes are not included. There is a `votes.json` per session; resolve their paths from [`latest.json`](latest.json)'s `sessions[]`. The structure is described by that session's `votes.schema.json`.

```jsonc
{
  "metadata": { /* source, generatedAt, session, totalVotes */ },
  "votes":       { "<voteId>": { /* roll call details */ } },
  "memberVotes": { "<personId>": [ { "voteId": "...", "vote": "Yea" }, ... ] }
}
```

- **Join:** members are joined to their votes by `id` — the same `id` used in `data/all.json`, `house.json`, and `senate.json`. `voteId` looks up the roll call in `votes`.
- **`vote`** is `"Yea"` or `"Nay"` when the member voted. Other values (for example `"Excused"` or `"Not Voting"`) mean they did not cast a vote.
- **`chamber` and `rollNumber`** are recorded by the source for each roll call. Use `chamber` rather than inferring it from the motion text: an "Agree to Senate Substitute" is a House vote. `rollNumber` is the chamber's own roll-call number and counts roll calls this file leaves out, so it is not dense here.
- **A bundled calendar vote appears once per bill,** with identical tallies, so counting rows overcounts floor votes.
- `metadata` records the source, when the file was generated, and the session it covers.

## Data sources & updates

All of the data comes from the Georgia General Assembly's own web service (legis.ga.gov). VoteGA.org adds a small editorial layer on top for the things that service doesn't record, mainly departures and vacancies, and keeps the cross-reference to Open States person ids described above.

The roster files (`data/*`) are refreshed daily, and the vote files and scorecard weekly. If an update fails, the files are left as they were rather than overwritten with incomplete data, so they may occasionally be a few days older than the schedule implies. When a new two-year session begins, the previous session's directory is left untouched and a new `sessions/<slug>/` is created.

## Reporting Corrections

Please do not edit data files directly. Open an issue using the **Data Correction** template and describe the inaccuracy. Corrections are reviewed, applied upstream at VoteGA.org, and automatically reflected here on the next daily run.

## License

[CC-BY-SA 4.0](https://github.com/Votega/ga-legislators?tab=CC-BY-SA-4.0-1-ov-file)
