# Decisions Register

<!-- Append-only. Never edit or remove existing rows.
     To reverse a decision, add a new row that supersedes it.
     Read this file at the start of any planning or research phase. -->

| # | When | Scope | Decision | Choice | Rationale | Revisable? |
|---|------|-------|----------|--------|-----------|------------|
| D001 | M001 | scope | Coherency analysis mode | Always included in full runs | User chose "always included" over separate `/perspective coherency` command. Keeps the interface simple — one command does everything. | Yes — if full runs feel too slow |
| D002 | M001 | scope | Quick mode trigger | `/perspective quick <topic>` prefix keyword | Cleanest heuristic to distinguish quick questions from focus areas. "quick" is explicit and unambiguous. | No |
| D003 | M001 | scope | Report format for coherency | Same report, new Coherency section | User chose single report over separate file. Everything in one place. | No |
| D004 | M001 | scope | OSS packaging level | Minimal — LICENSE, README, metadata only | No contributor ceremony until people actually contribute. Avoids premature process. | Yes — if contributors appear |
| D005 | M001 | arch | Coherency stage position | New stage after Research, before existing Audit | Coherency is about the codebase's internal structure. Logically sits between understanding the landscape and auditing against external docs. | Yes — if ordering causes issues |
