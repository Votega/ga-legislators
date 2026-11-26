# Changes

Changes to this repo's files that a consumer may need to act on. Routine data refreshes are not listed.

## Coming: 11 January 2027, new General Assembly

- `data/all.json`, `house.json`, `senate.json`, `members.csv` and `ROSTER.md` become the **2027-2028** roster. Each file's `metadata.session` (and the `session` column in `members.csv`) says which General Assembly it describes.
- `latest.json` names 2027-2028: `biennium`, `currentSession`, `sessions[]`. The 2025-2026 regular and 2026 special sessions move to `earlierBienniums[]`; their folders under `sessions/` do not move.
- `rosterArchive` becomes `null` until the 2027-2028 roster is frozen at sine die in 2028. The frozen 2025-2026 roster stays at `sessions/2025-2026/members.json` and is listed under `earlierBienniums[]`.
- `scorecard-latest.json` points at the 2027-2028 scorecard, which will rank few members in its first weeks. The final 2025-2026 scorecard stays at `sessions/2025-2026/scorecard.*` and is listed under its `earlierBienniums[]`.

## November 2026

- `latest.json` and `scorecard-latest.json` gained `earlierBienniums[]`, newest first. It indexes the 2017-2024 backfill, which was in `sessions/` but not indexed before.
- `rosterArchive` is `null` while the current General Assembly has no frozen roster, instead of naming a file that does not exist yet.
- `data/all.json` no longer holds the statewide executives.
