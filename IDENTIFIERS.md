# Identifiers in this dataset

> **Follow a person across sessions with `legisGaGovId`.** It is the only identifier that is present for every legislator in every session. The others are cross-references, each with limits listed below.

A session's `votes.json` keys `memberVotes` by the member `id`; look that `id` up in the same session's `members.json` to get the person's `legisGaGovId`.

## `legisGaGovId`

- **Carries across sessions:** Yes -- the only id that is
- **Type:** integer
- **What it is:** The General Assembly's own member number.
- **Source:** legis.ga.gov web services (Member.Id on every roll-call row and roster row). Stable back to 2001.
- **Where it appears:** Every roster row and every session's members.json/members.csv; the live roster; id-crosswalk.json. Votes carry it inside the member `id` only as the `legis-<id>` form, so join through members.json.
- **Coverage:** Every legislator who ever voted. No gaps. Use this to follow a person across sessions.

## `id (roster id)`

- **Carries across sessions:** No -- do not join sessions on it
- **Type:** string, opaque
- **What it is:** The join key between a session's votes.json memberVotes and its members.json. Do not read meaning into its shape.
- **Source:** `ocd-person/<uuid>` is inherited from Open States for members it catalogued. Otherwise `legis-<legisGaGovId>`, which the site builds.
- **Where it appears:** memberVotes keys in votes.json; `id` in members.json/members.csv; live ga-members.json and ga-member-votes.json.
- **Coverage:** Every roster row has one, but the form differs by person, and the same person can carry a different form in another file or session.

## `ocdPersonId`

- **Carries across sessions:** No
- **Type:** string or null
- **What it is:** Open States' person id.
- **Source:** Open States (the site no longer uses it as its source of record).
- **Where it appears:** Live ga-members.json and id-crosswalk.json only. Not in the session archive files.
- **Coverage:** Null for anyone Open States never catalogued.

## `vgId`

- **Carries across sessions:** Yes where it exists, but not yet published per session
- **Type:** string, e.g. vg-ga-p-000001
- **What it is:** The site's own person id. Append-only ledger: once minted it is never reassigned to another person.
- **Source:** Minted by the site's crosswalk builder, keyed on legisGaGovId for state legislators.
- **Where it appears:** id-crosswalk.json only.
- **Coverage:** Every current state legislator. Not minted for members who left before the site began tracking them. Absent from session files.

## `ballotpediaId`

- **Carries across sessions:** No
- **Type:** string or null
- **What it is:** The person's Ballotpedia page name.
- **Source:** Ballotpedia, matched by the site.
- **Where it appears:** Live ga-members.json and id-crosswalk.json only.
- **Coverage:** Partial.

## `legislationId`

- **Carries across sessions:** Unique within the legislature's system
- **Type:** integer
- **What it is:** The legislature's bill/resolution id.
- **Source:** legis.ga.gov web services.
- **Where it appears:** billUrl in votes; ga-bills.json.
- **Coverage:** Every vote record.

## `voteId`

- **Carries across sessions:** Unique within the legislature's system
- **Type:** string `<voteId>-<legislationId>`
- **What it is:** One roll call exploded to one record per bundled bill.
- **Source:** legis.ga.gov web services, combined by the site.
- **Where it appears:** votes keys in votes.json; the voteId column of votes.csv.
- **Coverage:** Every vote record.
