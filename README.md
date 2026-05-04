# ga-legislators

Machine-readable data for all current members of the Georgia General Assembly, maintained by [VoteGA.org](https://votega.org).

Updated automatically each time VoteGA.org's daily workflow runs.

---

## Files

| File | Contents |
|---|---|
| `data/all.json` | All 236 members (House + Senate) |
| `data/house.json` | House of Representatives only (180 seats) |
| `data/senate.json` | Senate only (56 seats) |

---

## Schema

Each member record is a JSON object with the following fields:

| Field | Type | Description |
|---|---|---|
| `id` | string | Open Civic Data person ID (`ocd-person/...`) — stable primary key |
| `name` | string | Full name |
| `firstName` | string | Given name |
| `lastName` | string | Family name |
| `party` | string | `"Republican"` or `"Democratic"` |
| `chamber` | string | `"House of Representatives"` or `"Senate"` |
| `district` | integer | District number |
| `title` | string | `"Representative"` or `"Senator"` |
| `imageUrl` | string | Official portrait URL (may be empty) |
| `phone` | string | Capitol office phone |
| `address` | string | Capitol office address |
| `email` | string | Official email address |
| `officialWebsiteUrl` | string | `legis.ga.gov` member page |
| `birthDate` | string | ISO date (`YYYY-MM-DD`), if available |
| `birthYear` | integer | Birth year, derived from `birthDate` |
| `termStart` | string | ISO date current term began, if known |
| `termStartYear` | integer | Year current term began, if known |
| `leadershipRole` | string | Leadership title, if applicable (e.g. `"Speaker of the House"`) — omitted when not applicable |

### Departure fields

Members who have left office mid-term may include the following additional fields:

| Field | Type | Description |
|---|---|---|
| `status` | string | Reason for departure: `"Resigned"`, `"Suspended"`, `"Removed"`, `"Deceased"`, or `"Vacant"` |
| `statusDate` | string | ISO date (`YYYY-MM-DD`) the change took effect |
| `statusNote` | string | Optional detail — e.g. executive order reference, appointment reason |

These fields are omitted for members who are currently serving. Seats with a departure status may be subject to a special election.

---

## Data Source and Freshness

Member data is sourced from the [Open States API](https://openstates.org/) (Plural Policy), a nonpartisan nonprofit that standardizes legislative data across all 50 states. VoteGA.org fetches this data daily and applies a manual override layer to fill in term start dates, leadership roles, and departure status that Open States does not reliably provide for Georgia.

**Update schedule:** Daily, after the VoteGA.org GA member workflow runs.

---

## Contributing

Direct edits to data files are not accepted via pull request — the files are overwritten on each daily run.

To report a correction (wrong phone number, missing email, outdated role, etc.), open an issue using the **Data Correction** template. Corrections are reviewed and applied upstream at VoteGA.org, then flow back to this repository automatically on the next update.

---

## License

[GPL-3.0](LICENSE)
