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

## Comment 3 - Missing test

**What I did:** Created `tests/test_watchlist.py`, mirroring the fixture
structure of `test_collection.py` (`app`, `sample_user`, `sample_film`).
Added three tests: `test_add_to_watchlist_creates_entry` (happy path),
`test_add_to_watchlist_duplicate_raises` (validates the Comment 2 dedup fix),
and `test_add_to_watchlist_nonexistent_film_raises` (the test the reviewer
asked for). Went beyond the literal ask since CONTRIBUTING.md requires 3
tests minimum for any new service function, and `add_to_watchlist()` had zero
coverage before this. One adaptation from the `test_collection.py` pattern:
the fake nonexistent film_id is an integer (`999999`), not a UUID string,
since `Film.id` is still `Integer` on this branch (pre-rebase).
**How I verified:** Ran `pytest tests/test_watchlist.py -v` - all 3 pass.
Ran the full suite `pytest tests/ -v` - 7 passed, no regressions.

## Comment 4 - Default visibility

**My position:** Watchlist entries should default to private (public=False),
not public=True.
**Reasoning:** A watchlist is a personal "want to watch" list - it reflects
someone's private taste/interests before they've actually watched, unlike a collection (already-watched films). Defaulting to
private respects that it's personal, and lets a user actively choose to
share it (e.g. with friends) rather than assuming everyone wants their
watchlist exposed.
**Tradeoff acknowledged:** CineLog is described as a community app, and
public watchlists could support discovery/recommendations between users.
But I don't think private-by-default actually costs the app that community
value - sharing/comments/recommendations can still happen, just as an
explicit choice (via the new `public` param) instead of an invisible
default. That's arguably a better fit for the reviewer's actual concern:
"I want to make sure we're being intentional, not just inheriting a
default" - an explicit opt-in is the most intentional version of this.
I implemented this by changing `WatchlistEntry.public`'s default to `False`
and adding an explicit `public` parameter to `add_to_watchlist()` so
callers/routes decide visibility on purpose rather than inheriting a
silent default.

## Comment 5 - Sort order

**My position:**
**Reasoning:**
**Engagement with reviewer's point:**

## Comment 6 — Rebase

**What conflicted:**
**How I resolved it:**
**How I verified no conflict remains:**

## PR Description

<!-- Written at the end — feature overview, design decisions, manual testing steps -->
