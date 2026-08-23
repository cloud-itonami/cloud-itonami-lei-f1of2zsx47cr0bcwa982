# cloud-itonami-lei-f1of2zsx47cr0bcwa982

> **Independent third-party archive/analysis. Not affiliated with, endorsed by, or sponsored by Carnival Corporation Ltd..**

This repository archives the publicly published Terms of Use / Terms and Conditions of
**Carnival Corporation Ltd.**, with source-url and retrieval-date provenance, per
[ADR-2607110300](https://github.com/com-junkawasaki/root/blob/main/90-docs/adr/2607110300-cloud-itonami-lei-corporate-tos-catalog.md)
(`cloud-itonami-lei-corporate-tos-catalog`, `com-junkawasaki/root`). It is a read-only
reference/archive repository — it does not act, propose, or execute anything on the
company's behalf, and is not a governed Advisor/Governor actor.

## Company identity

- **Legal name**: Carnival Corporation Ltd.
- **LEI (ISO 17442)**: [F1OF2ZSX47CR0BCWA982](https://search.gleif.org/#/record/F1OF2ZSX47CR0BCWA982) (GLEIF-verified)
- **Jurisdiction**: BM
- **Website**: https://www.carnival.com
- **Ticker**: CCL (NYSE)

## Contents

- `80-data/public/tos.journal.edn` — EDN quad-log of archived Terms of Use documents,
  each entry carrying `:tos/full-text`, `:tos/source-url`, `:tos/retrieved-at`,
  `:tos/sha256`, `:tos/doc-type`, and a `:tos/supersedes` chain for future revisions.
- `facts.edn` — 10 verified registry facts with per-fact provenance. **Generated** — see below.
- `scripts/verify-facts.cljs` — re-fetches every source `facts.edn` cites and fails if
  the live record disagrees. Vendored from `com-junkawasaki/root`
  (`scripts/lei-verify-facts.cljs`); fix issues in the canonical and re-vendor.
- `NOTICE` — copyright/attribution statement for the archived third-party text.
- `blueprint.edn` — machine-readable company identity record.

## Verifying the record

The LEI claims above used to be assertions with nothing in the repository behind
them. `facts.edn` now carries them as data, and every value in it was read out of
a public registry response whose URL and retrieval time sit next to the value:

```
nbb scripts/verify-facts.cljs           # check the recorded facts against the live sources
nbb scripts/verify-facts.cljs --write   # re-fetch and rewrite facts.edn
```

Eleven GLEIF/ISO URLs were fetched and ten facts recorded — the LEI record
(legal name as GLEIF spells it, **`CARNIVAL CORPORATION LTD.`**; entity **ACTIVE**,
registration **ISSUED**, `FULLY_CORROBORATED` / `CONFORMING`; entity status and
registration status are different fields and are recorded separately; legal and
headquarters address both `C/O CONYERS CORPORATE SERVICES (BERMUDA) LIMITED,
CLARENDON HOUSE, 2 CHURCH STREET, HM 11, HAMILTON` as GLEIF spells it; entity
creation date `1974-11-21`), its **136 ISINs** (a count read from
`meta.pagination.total` of the cited page — at this issuer's volume the
individual instruments are deliberately *not* mirrored, because they turn over
as notes mature and are issued, which would make the check red for reasons that
are not "the citation broke"; walk the cited URL's ten pages to enumerate them),
its managing LOU and LEI-issuer accreditation (London Stock Exchange LEI
Limited), registration authority `RA000028` (the Bermuda Companies Register kept
by the Registrar of Companies, entry `202605772`), ISO 20275 legal form `7AS7`
(Bermuda exempted company limited by shares), reporting exceptions at both
consolidation levels (`NO_KNOWN_PERSON` — GLEIF's code for an entity with no
known controlling person, so there is no parent to report), and **one direct
child** recorded as its own entity: Carnival UK Ltd. (GB). Nine of the eleven
URLs answered `200` when the file was written; the `direct-parent` and
`ultimate-parent` endpoints answered `404` because GLEIF publishes the exception
side of that pair for this entity, which the checker treats as a fact rather
than a failure.

One thing the cited LEI record says that `facts.edn` does not yet carry: GLEIF
lists `CARNIVAL CORPORATION.` as this entity's **previous legal name**
(`otherNames`, type `PREVIOUS_LEGAL_NAME`), and the Bermuda register entry
`202605772` sits next to a registration last updated `2026-06-02`. The generator
records neither previous names nor the update that introduced them, so read
those at the cited URL, not from this file. Extending the canonical generator is
the place to fix that — not this copy.

The checker exits `0` only when every cited URL answered and every recorded
value still matches. It exits `1` when a value drifted (naming the fact and the
key), a cited URL broke, or an entity appeared or disappeared; and it exits `3`
— refusing to report a pass — when it could not run at all (no egress, or no
`facts.edn` to check). Before landing it was shown to do all three against the
live API: unmodified → `0`; `:company/jurisdiction` edited `BM` → `PA` → `1`
naming `gleif-lei-record :company/jurisdiction`; the direct-child entity deleted
→ `1` naming it as `ADDED`; DNS cut → `3`; `facts.edn` absent → `3`.

`facts.edn` is tx-data: `(d/transact conn (edn/read-string (slurp "facts.edn")))`
loads it like every other EDN corpus in this workspace, with `:company/lei` as the
join key and `:source/dataset "cloud-itonami-lei-facts"` as its provenance.

## Design rationale

See ADR-2607110300 in `com-junkawasaki/root` (`90-docs/adr/`) for why this repo exists,
why it is keyed by LEI rather than GTIN or ticker, and why full-text archival (with
provenance) was chosen over excerpt-only storage.
