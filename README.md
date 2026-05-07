# ga-legislators

Current data for all members of Georgia's General Assembly, maintained by [VoteGA.org](https://votega.org) and automatically refreshed daily.

## Data Files

| File | Contents |
|---|---|
| `data/all.json` | All members (House + Senate combined) |
| `data/house.json` | House of Representatives members only |
| `data/senate.json` | Senate members only |

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

## Data Sources & Updates

Member data originates from the [Open States API](https://openstates.org/) (Plural Policy), a nonpartisan nonprofit that standardizes legislative data across all 50 states. VoteGA.org supplements this with a manual override layer to fill in term start dates, leadership roles, departure status, and other Georgia-specific details that Open States doesn't reliably capture.

This repo is updated automatically each time VoteGA.org's daily workflow runs.

## Reporting Corrections

Please do not edit data files directly. Open an issue using the **Data Correction** template and describe the inaccuracy. Corrections are reviewed, applied upstream at VoteGA.org, and automatically reflected here on the next daily run.

## License

GPL-3.0
