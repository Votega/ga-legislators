# ga-legislators

Data for all members of Georgia's General Assembly, maintained by [VoteGA.org](https://votega.org). The **current roster** refreshes daily; **voting records** refresh weekly. A General Assembly is a two-year *biennium* — a regular session plus any special sessions the Governor convenes — and the upstream data is replaced wholesale over time, so this repo keeps a permanent **per-session archive**: each session's votes (and the frozen end-of-biennium roster) live in their own folder and are never lost.

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

## Schema

Each member record includes the following fields:

### Identity & Position
| Field | Type | Description |
|---|---|---|
| `id` | string | Open States person ID (`ocd-person/…`) |
| `legisGaGovId` | integer \| null | Numeric ID on legis.ga.gov — used to link to voting history and official member pages |
| `name` | string | Full display name |
| `firstName` | string | Given name |
| `lastName` | string | Family name |
| `party` | string | `"Republican"`, `"Democratic"`, or `"Independent"` |
| `chamber` | string | `"House of Representatives"` or `"Senate"` |
| `district` | integer | District number |
| `title` | string | `"Representative"` or `"Senator"` |
| `leadershipRole` | string \| null | e.g. `"Speaker of the House"`, `"Majority Leader"` |

### Contact
| Field | Type | Description |
|---|---|---|
| `phone` | string | Capitol office phone |
| `email` | string | Official email address |
| `address` | string | Capitol office mailing address |
| `officialWebsiteUrl` | string | Member page on legis.ga.gov |
| `imageUrl` | string | Portrait image URL from legis.ga.gov; empty string if unavailable |

### Biographical
| Field | Type | Description |
|---|---|---|
| `birthDate` | string | ISO date (`YYYY-MM-DD`) |
| `birthYear` | integer \| null | Birth year extracted from `birthDate` |
| `termStart` | string | ISO date of current term start |
| `termStartYear` | integer \| null | Year extracted from `termStart` |

### Committees
| Field | Type | Description |
|---|---|---|
| `committees` | string[] | Committee names the member currently serves on |

### Departure (mid-term vacancies)
Members who left office mid-term include additional fields:

| Field | Type | Description |
|---|---|---|
| `status` | string | `"Resigned"`, `"Removed"`, `"Suspended"`, `"Deceased"`, or `"Vacant"` |
| `statusDate` | string | Effective date of departure (ISO date) |
| `statusNote` | string \| null | Context or source link (may contain markdown) |

## Voting Records (`sessions/<slug>/votes.json`)

Passage votes — final up-or-down floor votes on a bill — for one session of the General Assembly, with how every member voted on each one. Procedural motions, amendments, and committee votes are not included. There is a `votes.json` per session; resolve their paths from [`latest.json`](latest.json)'s `sessions[]`.

Members are joined to their votes by `id` — the same Open States person ID (`ocd-person/…`) used throughout `data/all.json`, `house.json`, and `senate.json`.

### Structure

```jsonc
{
  "metadata": { /* see below */ },
  "votes":       { "<voteId>": { /* roll call details */ } },
  "memberVotes": { "<personId>": [ { "voteId": "...", "vote": "Yea" }, ... ] }
}
```

### `votes[voteId]`

| Field | Type | Description |
|---|---|---|
| `bill` | string | Bill identifier (e.g. `"SB 29"`) |
| `billUrl` | string | Link to the bill on legis.ga.gov |
| `title` | string | Bill title |
| `session` | string | Session id this roll call belongs to (`"2025_26"` regular, `"2026_ss"` special). All roll calls in one `sessions/<slug>/votes.json` share it. |
| `motionText` | string | Roll call description (e.g. `"Senate Vote #133 - 2025-2026 Regular Session"`) |
| `date` | string | ISO date the vote was taken |
| `yea` / `nay` | integer | Chamber-wide yea/nay counts |
| `result` | string | `"Pass"` or `"Fail"` |

### `memberVotes[personId]`

An array of `{ "voteId": string, "vote": string }` — one entry per roll call that member participated in. `voteId` looks up the roll call in `votes` above.

`vote` is one of:

| Value | Meaning |
|---|---|
| `"Yea"` | Voted in favor |
| `"Nay"` | Voted against |
| `"Other"` | Absent, excused, not voting, or present — Open States collapses these into a single category for Georgia |

### `metadata`

| Field | Description |
|---|---|
| `generatedAt` | ISO timestamp of this file's generation |
| `session` / `sessionName` | Legislative session covered |
| `totalVotes` | Number of roll calls in `votes` |
| `totalBillsSeen` | Number of bills the generator paginated through |
| `paginationComplete` | `false` if a refresh didn't finish fetching every bill before stopping |
| `duplicateVotesDropped` | Roll-call entries removed because a member appeared twice for the same vote (an upstream data artifact) |
| `crossChamberDropped` | Entries removed because a member was erroneously attributed a vote from the chamber they don't belong to |

## Data Sources & Updates

Member data originates from the [Open States API](https://openstates.org/) (Plural Policy), a nonpartisan nonprofit that standardizes legislative data across all 50 states. VoteGA.org supplements this with a manual override layer to fill in term start dates, leadership roles, departure status, and other Georgia-specific details that Open States doesn't reliably capture.

`data/all.json`, `house.json`, and `senate.json` are updated automatically each time VoteGA.org's daily member-data workflow runs. The current session's `sessions/<slug>/votes.*` files (and `latest.json`) are updated each time VoteGA.org's weekly vote-data workflow runs — if that workflow fails (e.g. an upstream API outage), the files simply aren't touched that week rather than being overwritten with incomplete data, so they may occasionally be a few days staler than the weekly cadence implies. When a new two-year session begins, the previous session's directory is left in place untouched, and a new `sessions/<slug>/` is created for the incoming session.

## Reporting Corrections

Please do not edit data files directly. Open an issue using the **Data Correction** template and describe the inaccuracy. Corrections are reviewed, applied upstream at VoteGA.org, and automatically reflected here on the next daily run.

## License

GPL-3.0
