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
only two occurrences existed - the definition and one call site — both updated.
Ran `pytest tests/ -v` afterward; all 4 existing tests still passed.

## Comment 2 - Deduplication

**What I did:** Added `AlreadyInWatchlistError` to `services/watchlist_service.py`
and a dedup check in `add_to_watchlist()` - queries for an existing
`WatchlistEntry` with the same `user_id`/`film_id` before creating a new one,
raising `AlreadyInWatchlistError` if found. Mirrors the pattern in
`add_to_collection()`. Also updated `routes/watchlist/watchlist.py`'s `add_film`
route to catch both `FilmNotFoundError` (404) and `AlreadyInWatchlistError`
(409), matching `routes/collection.py`'s error handling - previously neither
exception was caught there, so both cases would have 500'd.
**How I verified:** Ran `pytest tests/ -v` - all 4 existing tests still pass.
(No automated test for the duplicate case yet - will add one in Comment 3/test coverage work.)

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
