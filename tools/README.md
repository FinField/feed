# Feed tooling

The exact tooling that produced this corpus (from the archived `FinField/finfield` monolith, final state incl. the O(n) feed-rebuild fix for Knitweb/pulse#338). `bulk_publish.py` streams the SEC companyfacts.zip and appends signed records idempotently (per-CID); `publish.py` holds the FeedStore/Publisher (sharded log, signed head, relay announce, serve). Successor code lives in the modular repos (finscrapers `sec_bulk`, finknit, finagents); kept here so the corpus stays reproducible byte-for-byte.
