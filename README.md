# Octra HFHE Challenge v2 — Public Audit Log

This repository tracks a public, reproducible audit of the **Octra HFHE Challenge v2** artifacts. It is maintained as a clean publication copy of local investigation notes, selected evidence outputs, and negative-result checks.

> Current status: **not solved**. No plaintext, wallet private key, `PRF_R2`, `PRF_R3`, `sk.prf_k`, Toeplitz stream material, or PC opening has been recovered from public material.

## Target wallet

```text
octC5eR9pLGKbpzTbDgHowkFt8HW7LZYb2gzehzxHamxuAZ
```

Observed public-chain state in the latest local refresh:

```text
balance:        500001.000001
nonce:          0
has_public_key: false
tx_count:       5
```

## Purpose

The goal is to document what has been tested against the public challenge package and to avoid repeatedly re-running already-negative routes. This repository contains:

- latest progress summaries;
- selected reproducible audit outputs;
- public external-lead triage;
- blocked-conclusion notes;
- scripts and helper material that do not contain local secrets.

It intentionally excludes local tokens, `github.txt`, unrelated wallet files, private scratch data, and unverified secret material.

## Main conclusion so far

The published challenge material exposes useful metadata and `pvac.prf.r.1` LPN side samples, but decryption of a layer requires the full mask product:

```text
R = PRF_R1 * PRF_R2 * PRF_R3
```

Current public material does **not** expose the missing components required to compute that product:

- `PRF_R2`
- `PRF_R3`
- `sk.prf_k`
- Toeplitz secret-derived stream material
- PC openings (`value` / `blind`)
- plaintext
- target wallet private key

The strongest current blocker is therefore not parsing the artifact, but the absence of a public verifier or leak for the missing secret-derived mask material.

## Key negative checks already recorded

- `secret.ct` parses as a valid wrapped-v2 bundle: 22 ciphertexts, 44 base layers, no trailing bytes.
- No appended hidden PVAC/OCTRA block or ASCII plaintext pocket was found.
- No repeated `RSeed`, near-duplicate nonce, zero seed, zero PC point, or duplicate PC point was found.
- `pk.bin` public matrix `H` has no zero/duplicate columns and is full row rank over GF(2).
- Public `Layer::PC` entries are commitment points only; opening material is not serialized in `secret.ct`.
- Public LPN samples are bound to `pvac.prf.r.1` only; source-level audit confirms `PRF_R2` and `PRF_R3` remain independent domain calls under `sk.prf_k`.
- External social/GitHub leads checked so far are negative or non-verifiable, including the Kubo/smoke-ui Day 6 algebraic-track report.

## Repository layout

```text
status_latest.md       Latest progress snapshot
README.md              This overview
docs/                  Human-readable explanations and reproduction notes
report/                Current blocked conclusion
audits/                Selected audit helpers
evidence/              Selected sanitized output logs
scripts/               Reproduction helper scripts
repo_metadata.json     Publication metadata
```

Useful entry points:

- [`status_latest.md`](./status_latest.md)
- [`docs/PROJECT_SUMMARY_ZH.md`](./docs/PROJECT_SUMMARY_ZH.md)
- [`docs/INTRODUCTION.md`](./docs/INTRODUCTION.md)
- [`docs/FINDINGS.md`](./docs/FINDINGS.md)
- [`docs/REPRODUCTION.md`](./docs/REPRODUCTION.md)
- [`report/HFHE_V2_BLOCKED_CONCLUSION_CURRENT.md`](./report/HFHE_V2_BLOCKED_CONCLUSION_CURRENT.md)

## External references triaged

- [`smoke-ui/octra-hfhe-v2-security-assessment`](https://github.com/smoke-ui/octra-hfhe-v2-security-assessment) — independent negative-result baseline; no target recovery.
- [`tomismeta/octra-sqlite`](https://github.com/tomismeta/octra-sqlite) — useful Octra wallet-format reference; no target/challenge leak found.
- Public X posts and GitHub PRs/forks related to the challenge — monitored for verifiable artifacts or candidate secrets.

## Validation rule for any future candidate

Any future plaintext/private-key candidate must be validated locally before being treated as progress:

1. candidate decrypts or verifies against local `secret.ct` / `pk.bin`, or derives the target address;
2. candidate is consistent with the target wallet state;
3. result is reproducible from recorded steps;
4. no unrelated local secret or token is included in publication.

Until such validation exists, the public status remains **not recovered**.
