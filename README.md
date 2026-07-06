# FinField bootstrap feed

**93,813 signed, content-addressed financial facts for 6,500 SEC reporters** — the FinField field's bootstrap corpus. Produced with the FinField pipeline ([facts](https://github.com/FinField/facts) model → SEC EDGAR companyfacts → [knit](https://github.com/FinField/knit) plugin), published as an append-only feed so the data lives on the network's nodes and mirrors, never on one machine.

## Layout

```
feed/records-NNNNN.jsonl   append-only shards (20k records each), one canonical JSON record per line
feed/head.json             signed FeedHead (secp256k1) over the whole history
feed/MANIFEST.json         publisher address + shard list + record count
```

Publisher: `pls1adzrkahczs3yqcfeciuwwnnkbcmy3sbl6e` (head root `81af9301a4ac…`, length 93,813).

## What's in the core pack

Latest observation per concept per company, from audited XBRL filings (US-government public domain), each with its SEC accession number:

- shares outstanding and **free float** (`dei:EntityPublicFloat`) — date-tied integers
- income statement: revenues, operating/net income, opex, cost of revenue
- balance sheet: assets, liabilities, stockholders' equity, cash
- capex, EPS basic/diluted

Values are exact scaled integers (`value × 10⁻ˢᶜᵃˡᵉ`), never floats. Entity records (`finfield-entity`) carry name/CIK/ticker/asset class.

## Verify, then trust

- head signature → publisher authenticity
- record CID (`canonical.cid(record)`, CIDv1 dag-cbor) → content integrity
- `source.ref` → the SEC filing the number came from
- `author` field + attestation → non-repudiable authorship

## Updates

Publishing is idempotent per CID: each re-ingest of new filings appends only the delta plus a new signed head. The [agents](https://github.com/FinField/agents) fleet takes over continuous knitting on the field nodes; history deltas and non-US sources (ESEF, EDINET, …) extend this corpus.
