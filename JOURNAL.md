## Week 7 — Issue selection

**Issue link:** https://github.com/ascherj/pathreview/issues/13

**Issue title:** Add a content hash to detect unchanged documents and skip re-embedding

**Tier:** [ ] Tier 1  [*] Tier 2  [ ] Tier 3

**Problem summary:**
The ingestion pipeline re-embeds every resume, README, and repo submission from scratch on every ingestion call, even when the content is byte-for-byte identical to something already processed. The dedup logic already existed in shape — a content hash was computed and folded into `source_id`, and the `IngestedSource` model even had an unused `content_hash` column — but the wiring was never finished: the skip-check queried a placeholder instead of the real database model and always returned nothing, and the record-keeping step only logged instead of writing to the database. On top of that, the database session is async-only but the pipeline's methods were synchronous, so even a corrected query couldn't have worked as written. A successful fix detects when resubmitted content is unchanged and skips parsing, chunking, and embedding entirely, cutting unnecessary embedding-API calls and redundant vector-store writes without touching the case where content actually changed.

**"Is this right for me?" checklist reasoning:**
_TODO — pending the actual checklist text from the course unit page (auth-walled, not yet available to fill in honestly)._

**Branch name:** feat/13-hashtodetect-unchange

**Setup confirmation:** [*] App runs locally at localhost:5173

**Cohort ledger:** [*] Issue added to cohort ledger