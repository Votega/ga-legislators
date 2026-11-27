# Changes

Changes to this repo's files that a consumer may need to act on. Routine data refreshes are not listed.

## Coming: 11 January 2027, new General Assembly

- `data/all.json`, `house.json`, `senate.json`, `members.csv` and `ROSTER.md` become the **2027-2028** roster. Each file's `metadata.session` (and the `session` column in `members.csv`) says which General Assembly it describes.
- `latest.json` names 2027-2028: `biennium`, `currentSession`, `sessions[]`. The 2025-2026 regular and 2026 special sessions move to `earlierBienniums[]`; their folders under `sessions/` do not move.
- `rosterArchive` becomes `null` until the 2027-2028 roster is frozen at sine die in 2028. The frozen 2025-2026 roster stays at `sessions/2025-2026/members.json` and is listed under `earlierBienniums[]`.
- `scorecard-latest.json` points at the 2027-2028 scorecard, which will rank few members in its first weeks. The final 2025-2026 scorecard stays at `sessions/2025-2026/scorecard.*` and is listed under its `earlierBienniums[]`.

## November 2026: final votes added; votes schema 1.1.0

52 bill-votes from 9 roll calls were missing: their captions did not start with "Passage", "Agree to" or "Adopt", or legis.ga.gov linked the roll call to no bill. They are added to `votes.json` (with every member's vote in `memberVotes`) and `votes.csv`. No existing vote, `voteId` or member vote changed.

| Bills | Chamber | Date | Roll call | Tally | `result` | File |
|---|---|---|---|---|---|---|
| HB 86 | Senate | 2025-03-28 | Recede from Senate Amendment | 46-2 | Pass | `sessions/2025-2026/` |
| 18 Senate resolutions (SR 216 … SR 489) | Senate | 2025-04-02 | Consent Calendar for Senate Study Committees | 52-0 | Pass | `sessions/2025-2026/` |
| 12 House resolutions (HR 40 … HR 887) | House | 2025-04-04 | Uncontested House Resolutions | 166-0 | Pass | `sessions/2025-2026/` |
| SB 234, SB 235, SB 336 | House | 2025-04-04 | SBs 234, 235, & 336 Passage | 147-2 | Pass | `sessions/2025-2026/` |
| SR 684, SR 744 | Senate | 2026-02-24 | Consent Calendar Expressing the Will of the Senate | 51-0 | Pass | `sessions/2025-2026/` |
| HB 973 | House | 2026-02-25 | Adopt CCR (conference report) | 169-2 | Pass | `sessions/2025-2026/` |
| SR 480, SR 567 | Senate | 2026-02-26 | Consent Calendar Expressing the Will of the Senate | 48-0 | Pass | `sessions/2025-2026/` |
| 12 Senate resolutions (SR 432 … SR 978) | Senate | 2026-03-31 | Consent Calendar for Senate Study Committees | 48-0 | Pass | `sessions/2025-2026/` |
| HB 1192 | House | 2026-06-22 | Veto Override | 94-77 | Fail | `sessions/2026-ss/` |

- **SB 234, SB 235, SB 336:** the 112-59 `Fail` listed in the next entry stands; the House reconsidered and the 147-2 re-vote passed them.
- **HB 1192** is a 2025-2026 bill, but the House took the override in the 2026 special session, so the vote is in `sessions/2026-ss/votes.json` and carries the new **`billSession: "2025_26"`**. An override needs two-thirds, 120 in the House.
- The scorecard is recomputed from the new votes on its next run.

Schemas: `schemaVersion` is now `1.1.0` on the members, votes and scorecard schemas. Only `votes.schema.json` changed: it describes `billSession` (optional, never `required`, so older copies still validate) and says `session` is the session that held the vote.

## November 2026: corrected vote results

A roll call now passes only with a majority of the chamber's seats, 91 of 180 in the House and 29 of 56 in the Senate (Ga. Const. Art. III, Sec. V, Para. V). Two kinds of measure need two-thirds of the members elected, 120 / 38: a resolution proposing a constitutional amendment (Art. X, Sec. I, Para. II), and a local act granting or changing a property-tax (homestead) exemption (Art. VII, Sec. II, Para. II(a); legis.ga.gov records every such act below 120 / 38 as lost). Before, `result` was `Pass` whenever yeas outnumbered nays, and the two-thirds rule was applied only to 2025-2026 amendments. Each vote below was published as `Pass`. legis.ga.gov's status history records it as lost ("Third Reading Lost", "Senate Lost"), or the chamber took it again the same day and the re-vote passed. Only `result` changed: tallies, member votes and voteIds are as they were.

Changed files: `sessions/2017-2018/`, `sessions/2019-2020/`, `sessions/2021-2022/`, `sessions/2023-2024/`, `sessions/2025-2026/` and `sessions/2026-ss/` `votes.json` and `votes.csv`. The 2017-2024 `votes.json` `metadata` also gains `resultRule` and has `amendmentRuleApplied: true`. The permanent `sessions/` folders were corrected in place, because the published results were wrong.

Four votes keep `Pass` below the floor because the official record says they passed: 2019-2020 HB 1029 (House 90-69) and HB 1030 (House 90-65), 2021-2022 HB 1331 (House 90-59), and 2017-2018 SB 317 (Senate 36-17). Two statewide exemption bills that the official record says lost are `Fail`: 2021-2022 SB 474 (Senate 31-17) and 2023-2024 HB 808 (Senate 33-18).

| Biennium | voteId | Bill | Chamber | Date | Tally | Now | Why |
|---|---|---|---|---|---|---|---|
| 2017-2018 | `14955-50033` | SR 104 | Senate | 2017-02-28 | 34-16 | Fail | two-thirds (120 / 38) not reached |
| 2017-2018 | `15309-49598` | SB 1 | House | 2017-03-28 | 85-83 | Fail | majority of seats (91 / 29) not reached |
| 2017-2018 | `15311-49598` | SB 1 | House | 2017-03-28 | 84-83 | Fail | majority of seats (91 / 29) not reached |
| 2017-2018 | `15501-49827` | HB 125 | Senate | 2017-03-30 | 27-14 | Fail | majority of seats (91 / 29) not reached |
| 2017-2018 | `15504-50310` | SB 133 | Senate | 2017-03-30 | 26-23 | Fail | majority of seats (91 / 29) not reached |
| 2017-2018 | `15652-51980` | SB 317 | House | 2018-02-14 | 107-50 | Fail | two-thirds (120 / 38) not reached |
| 2017-2018 | `15652-52082` | HB 707 | House | 2018-02-14 | 107-50 | Fail | two-thirds (120 / 38) not reached |
| 2017-2018 | `15652-52083` | HB 708 | House | 2018-02-14 | 107-50 | Fail | two-thirds (120 / 38) not reached |
| 2017-2018 | `15652-52085` | HB 710 | House | 2018-02-14 | 107-50 | Fail | two-thirds (120 / 38) not reached |
| 2017-2018 | `15652-52086` | HB 711 | House | 2018-02-14 | 107-50 | Fail | two-thirds (120 / 38) not reached |
| 2017-2018 | `15652-52087` | HB 712 | House | 2018-02-14 | 107-50 | Fail | two-thirds (120 / 38) not reached |
| 2017-2018 | `15652-52599` | HB 863 | House | 2018-02-14 | 107-50 | Fail | two-thirds (120 / 38) not reached |
| 2017-2018 | `15958-52927` | HB 945 | House | 2018-03-05 | 104-56 | Fail | two-thirds (120 / 38) not reached |
| 2019-2020 | `16573-54361` | HB 72 | House | 2019-02-25 | 82-72 | Fail | majority of seats (91 / 29) not reached |
| 2019-2020 | `17256-54694` | SB 72 | House | 2019-04-02 | 89-72 | Fail | majority of seats (91 / 29) not reached |
| 2019-2020 | `17313-55536` | HB 516 | Senate | 2019-04-02 | 27-25 | Fail | majority of seats (91 / 29) not reached |
| 2019-2020 | `17712-57726` | SR 818 | Senate | 2020-03-12 | 33-19 | Fail | two-thirds (120 / 38) not reached |
| 2019-2020 | `17726-57917` | SR 854 | Senate | 2020-03-12 | 26-22 | Fail | majority of seats (91 / 29) not reached |
| 2019-2020 | `17903-57131` | SB 344 | House | 2020-06-24 | 81-68 | Fail | majority of seats (91 / 29) not reached |
| 2019-2020 | `17933-56678` | HB 765 | Senate | 2020-06-24 | 28-19 | Fail | majority of seats (91 / 29) not reached |
| 2019-2020 | `17973-54684` | SB 71 | House | 2020-06-25 | 83-79 | Fail | majority of seats (91 / 29) not reached |
| 2021-2022 | `18489-59632` | SB 166 | Senate | 2021-03-08 | 26-24 | Fail | majority of seats (91 / 29) not reached |
| 2021-2022 | `18491-59632` | SB 166 | Senate | 2021-03-08 | 27-24 | Fail | majority of seats (91 / 29) not reached |
| 2021-2022 | `18628-60621` | HB 797 | House | 2021-03-25 | 108-56 | Fail | two-thirds (120 / 38) not reached |
| 2021-2022 | `18669-59620` | SB 156 | House | 2021-03-29 | 85-84 | Fail | majority of seats (91 / 29) not reached |
| 2021-2022 | `18679-60096` | SB 260 | House | 2021-03-29 | 87-75 | Fail | majority of seats (91 / 29) not reached |
| 2021-2022 | `19005-61083` | SR 363 | Senate | 2022-01-24 | 33-14 | Fail | two-thirds (120 / 38) not reached |
| 2021-2022 | `19044-61370` | HB 1029 | House | 2022-02-02 | 115-46 | Fail | two-thirds (120 / 38) not reached |
| 2021-2022 | `19044-61371` | HB 1030 | House | 2022-02-02 | 115-46 | Fail | two-thirds (120 / 38) not reached |
| 2021-2022 | `19044-61372` | HB 1031 | House | 2022-02-02 | 115-46 | Fail | two-thirds (120 / 38) not reached |
| 2021-2022 | `19059-61449` | HB 1062 | House | 2022-02-03 | 104-61 | Fail | two-thirds (120 / 38) not reached |
| 2021-2022 | `19291-61258` | HR 594 | House | 2022-03-03 | 112-30 | Fail | two-thirds (120 / 38) not reached |
| 2021-2022 | `19315-61902` | SB 474 | Senate | 2022-03-04 | 31-17 | Fail | statewide exemption; officially "Senate Lost" |
| 2021-2022 | `19415-62268` | HB 1424 | House | 2022-03-15 | 88-79 | Fail | majority of seats (91 / 29) not reached |
| 2021-2022 | `19483-59903` | SR 131 | Senate | 2022-03-15 | 33-20 | Fail | two-thirds (120 / 38) not reached |
| 2021-2022 | `19637-61936` | SB 479 | House | 2022-03-29 | 89-62 | Fail | majority of seats (91 / 29) not reached |
| 2021-2022 | `19735-59695` | SB 183 | House | 2022-04-01 | 83-55 | Fail | majority of seats (91 / 29) not reached |
| 2021-2022 | `19889-59811` | HB 517 | Senate | 2022-04-04 | 28-20 | Fail | majority of seats (91 / 29) not reached |
| 2021-2022 | `19900-62335` | HR 842 | Senate | 2022-04-04 | 35-19 | Fail | two-thirds (120 / 38) not reached |
| 2023-2024 | `19913-63481` | SR 3 | Senate | 2023-01-09 | 28-23 | Fail | majority of seats (91 / 29) not reached |
| 2023-2024 | `21103-64349` | HB 391 | Senate | 2023-02-28 | 37-14 | Fail | two-thirds (120 / 38) not reached |
| 2023-2024 | `21260-64378` | SR 140 | Senate | 2023-03-06 | 30-26 | Fail | two-thirds (120 / 38) not reached |
| 2023-2024 | `21678-63901` | HB 196 | Senate | 2023-03-29 | 28-26 | Fail | majority of seats (91 / 29) not reached |
| 2023-2024 | `23157-66036` | HR 780 | House | 2024-02-29 | 98-61 | Fail | two-thirds (120 / 38) not reached |
| 2023-2024 | `24332-65535` | HB 808 | Senate | 2024-03-20 | 33-18 | Fail | statewide exemption; officially "Senate Lost" |
| 2023-2024 | `24339-66622` | HB 1114 | Senate | 2024-03-20 | 28-22 | Fail | majority of seats (91 / 29) not reached |
| 2023-2024 | `24517-64264` | HB 353 | Senate | 2024-03-26 | 27-26 | Fail | majority of seats (91 / 29) not reached |
| 2023-2024 | `24567-64688` | SR 189 | House | 2024-03-28 | 88-84 | Fail | two-thirds (120 / 38) not reached |
| 2025-2026 | `25398-*` | HB 360 | Senate | 2025-03-28 | 26-24 | Fail | majority of seats (91 / 29) not reached |
| 2025-2026 | `25595-*` | SB 234, SB 235, SB 336 | House | 2025-04-04 | 112-59 | Fail | two-thirds (120 / 38) not reached |
| 2025-2026 | `25742-*` | SB 170 | Senate | 2026-01-28 | 27-22 | Fail | majority of seats (91 / 29) not reached |
| 2025-2026 | `26078-*` | HB 1324 | House | 2026-03-06 | 87-76 | Fail | majority of seats (91 / 29) not reached |
| 2025-2026 | `26154-*` | SB 568 | Senate | 2026-03-06 | 27-21 | Fail | majority of seats (91 / 29) not reached |
| 2025-2026 | `26464-*` | SB 175 | House | 2026-03-31 | 89-73 | Fail | majority of seats (91 / 29) not reached |
| 2025-2026 | `26558-*` | SB 614, SB 627 | House | 2026-04-02 | 94-75 | Fail | two-thirds (120 / 38) not reached |
| 2026 special | `26658-*` | HB 10EX, HB 11EX, HB 12EX, HB 13EX, HB 14EX, HB 15EX, HB 16EX, HB 17EX, HB 18EX, HB 19EX, HB 1EX, HB 20EX, HB 21EX, HB 22EX, HB 23EX, HB 24EX, HB 25EX, HB 26EX, HB 27EX, HB 28EX, HB 29EX, HB 2EX, HB 30EX, HB 31EX, HB 32EX, HB 33EX, HB 34EX, HB 35EX, HB 36EX, HB 37EX, HB 38EX, HB 39EX, HB 3EX, HB 40EX, HB 41EX, HB 42EX, HB 43EX, HB 44EX, HB 45EX, HB 46EX, HB 47EX, HB 48EX, HB 49EX, HB 4EX, HB 50EX, HB 51EX, HB 52EX, HB 53EX, HB 54EX, HB 55EX, HB 56EX, HB 57EX, HB 59EX, HB 5EX, HB 60EX, HB 61EX, HB 62EX, HB 63EX, HB 64EX, HB 65EX, HB 66EX, HB 67EX, HB 6EX, HB 7EX, HB 8EX, HB 9EX | House | 2026-06-20 | 95-67 | Fail | two-thirds (120 / 38) not reached |
| 2026 special | `26660-*` | SB 1EX, SB 2EX, SB 4EX | Senate | 2026-06-20 | 33-18 | Fail | two-thirds (120 / 38) not reached |
| 2026 special | `26668-*` | SB 1EX | Senate | 2026-06-20 | 33-18 | Fail | two-thirds (120 / 38) not reached |
| 2026 special | `26669-*` | SB 2EX | Senate | 2026-06-20 | 33-18 | Fail | two-thirds (120 / 38) not reached |
| 2026 special | `26670-*` | SB 4EX | Senate | 2026-06-20 | 33-15 | Fail | two-thirds (120 / 38) not reached |
| 2026 special | `26672-*` | HB 68EX, HB 70EX, HB 71EX, HB 72EX, HB 73EX, HB 74EX | House | 2026-06-22 | 97-74 | Fail | two-thirds (120 / 38) not reached |
| 2026 special | `26673-*` | HB 10EX, HB 11EX, HB 12EX, HB 13EX, HB 14EX, HB 15EX, HB 16EX, HB 17EX, HB 18EX, HB 19EX, HB 1EX, HB 20EX, HB 21EX, HB 22EX, HB 23EX, HB 24EX, HB 25EX, HB 26EX, HB 27EX, HB 28EX, HB 29EX, HB 2EX, HB 30EX, HB 31EX, HB 32EX, HB 33EX, HB 34EX, HB 35EX, HB 36EX, HB 37EX, HB 38EX, HB 39EX, HB 3EX, HB 40EX, HB 41EX, HB 42EX, HB 43EX, HB 44EX, HB 45EX, HB 46EX, HB 47EX, HB 48EX, HB 49EX, HB 4EX, HB 50EX, HB 51EX, HB 52EX, HB 53EX, HB 54EX, HB 55EX, HB 56EX, HB 57EX, HB 59EX, HB 5EX, HB 60EX, HB 61EX, HB 62EX, HB 63EX, HB 64EX, HB 65EX, HB 66EX, HB 67EX, HB 6EX, HB 7EX, HB 8EX, HB 9EX | House | 2026-06-22 | 97-75 | Fail | two-thirds (120 / 38) not reached |
| 2026 special | `26674-*` | HB 10EX, HB 11EX, HB 12EX, HB 13EX, HB 14EX, HB 15EX, HB 16EX, HB 17EX, HB 18EX, HB 19EX, HB 1EX, HB 20EX, HB 21EX, HB 22EX, HB 23EX, HB 24EX, HB 25EX, HB 26EX, HB 27EX, HB 28EX, HB 29EX, HB 2EX, HB 30EX, HB 31EX, HB 32EX, HB 33EX, HB 34EX, HB 35EX, HB 36EX, HB 37EX, HB 38EX, HB 39EX, HB 3EX, HB 40EX, HB 41EX, HB 42EX, HB 43EX, HB 44EX, HB 45EX, HB 46EX, HB 47EX, HB 48EX, HB 49EX, HB 4EX, HB 50EX, HB 51EX, HB 52EX, HB 53EX, HB 54EX, HB 55EX, HB 56EX, HB 57EX, HB 59EX, HB 5EX, HB 60EX, HB 62EX, HB 63EX, HB 64EX, HB 65EX, HB 66EX, HB 67EX, HB 6EX, HB 7EX, HB 8EX, HB 9EX | House | 2026-06-22 | 99-74 | Fail | two-thirds (120 / 38) not reached |
| 2026 special | `26677-*` | SB 5EX, SB 6EX, SB 7EX, SB 8EX, SB 9EX | Senate | 2026-06-22 | 31-20 | Fail | two-thirds (120 / 38) not reached |
| 2026 special | `26680-*` | HB 75EX, HB 76EX, HB 77EX, HB 78EX, HB 79EX, HB 80EX, HB 81EX, HB 82EX, HB 83EX | House | 2026-06-23 | 96-71 | Fail | two-thirds (120 / 38) not reached |
| 2026 special | `26681-*` | HB 75EX, HB 76EX, HB 77EX, HB 78EX, HB 79EX, HB 80EX, HB 81EX, HB 82EX, HB 83EX | House | 2026-06-23 | 97-72 | Fail | two-thirds (120 / 38) not reached |
| 2026 special | `26682-*` | HB 75EX, HB 76EX, HB 77EX, HB 78EX, HB 79EX, HB 80EX, HB 81EX, HB 82EX, HB 83EX | House | 2026-06-23 | 97-72 | Fail | two-thirds (120 / 38) not reached |
| 2026 special | `26683-*` | HB 68EX, HB 70EX, HB 71EX, HB 72EX, HB 73EX, HB 74EX | House | 2026-06-23 | 97-72 | Fail | two-thirds (120 / 38) not reached |
| 2026 special | `26684-*` | HB 68EX, HB 70EX, HB 71EX, HB 72EX, HB 73EX, HB 74EX | House | 2026-06-23 | 97-72 | Fail | two-thirds (120 / 38) not reached |
| 2026 special | `26690-*` | SB 5EX | Senate | 2026-06-23 | 32-19 | Fail | two-thirds (120 / 38) not reached |
| 2026 special | `26691-*` | SB 6EX | Senate | 2026-06-23 | 32-20 | Fail | two-thirds (120 / 38) not reached |

Rows marked `-*` are one roll call covering several bills (a local calendar); each bill's record `<rollcall>-<legislationId>` changed. 313 vote records changed in all. The scorecard and party-unity numbers do not use `result` and are unchanged.

## November 2026

- `latest.json` and `scorecard-latest.json` gained `earlierBienniums[]`, newest first. It indexes the 2017-2024 backfill, which was in `sessions/` but not indexed before.
- `rosterArchive` is `null` while the current General Assembly has no frozen roster, instead of naming a file that does not exist yet.
- `data/all.json` no longer holds the statewide executives.
