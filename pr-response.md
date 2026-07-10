# PR Response Doc - CineLog Watchlist Feature

## AI Usage

<!-- Fill in at the end — how you used AI tools during this project -->

## Comment 1 - Rename

**What I did:** Renamed `save_to_watchlist()` to `add_to_watchlist()` in
`services/watchlist_service.py`, and updated the import and call site in
`routes/watchlist/watchlist.py` to match. Now consistent with the project's
`verb_to_noun` convention (`add_to_collection`, `remove_from_collection`).
**How I verified:** Searched the project for all references to
`save_to_watchlist` (project-wide search / find-all-references) and confirmed
only two occurrences existed — the definition and one call site — both updated.
Ran `pytest tests/ -v` afterward; all 4 existing tests still passed.

## Comment 2 — Deduplication

**What I did:**
**How I verified:**

## Comment 3 — Missing test

**What I did:**
**How I verified:**

## Comment 4 — Default visibility

**My position:**
**Reasoning:**
**Tradeoff acknowledged:**

## Comment 5 — Sort order

**My position:**
**Reasoning:**
**Engagement with reviewer's point:**

## Comment 6 — Rebase

**What conflicted:**
**How I resolved it:**
**How I verified no conflict remains:**

## PR Description

<!-- Written at the end — feature overview, design decisions, manual testing steps -->
