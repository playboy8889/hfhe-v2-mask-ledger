# HFHE v2 Mask Ledger

A compact ledger of public-only checks around the Octra HFHE v2 challenge.

This repository is **not** a solved write-up and not a mirror of another audit repo. It is a running notebook focused on one question:

> Can the public files expose enough mask material to verify or recover the target wallet secret?

Current answer: **not yet**.

## Challenge anchor

```text
wallet: octC5eR9pLGKbpzTbDgHowkFt8HW7LZYb2gzehzxHamxuAZ
observed balance: 500001.000001 OCT
latest public status: nonce 0, has_public_key false, tx_count 5
```

## Working model

The useful public numerator for a base layer is masked. For a wrapped text block, recovery would require the hidden per-layer masks:

```text
plaintext block = public_numerator_0 / R0 + public_numerator_1 / R1
R = PRF_R1 · PRF_R2 · PRF_R3
```

The published side data covers `pvac.prf.r.1`. The ledger tracks whether any public route exposes the rest of the product or an equivalent verifier.

## Current ledger state

| Area | Current observation |
|---|---|
| wallet state | unchanged; no public-key reveal on target account |
| bundle framing | 22 ciphers, 44 base layers, no hidden trailing payload found |
| base seeds / nonces | no reuse or near-duplicate pattern observed |
| PC commitments | commitment points only; no serialized `value`/`blind` openings |
| public matrix | `H` full row rank, no duplicate/zero columns in current audit |
| LPN samples | useful only for `pvac.prf.r.1`; no `r.2`/`r.3` material observed |
| external leads | Kubo/smoke-ui/startkey/asbryx leads are negative public-artifact audits; no target secret material |

## Reading order

1. [`status_latest.md`](./status_latest.md) — last recorded event or external lead.
2. [`docs/NOTEBOOK_ZH.md`](./docs/NOTEBOOK_ZH.md) — Chinese notebook-style summary.
3. [`docs/MASK_MODEL.md`](./docs/MASK_MODEL.md) — why the missing masks matter.
4. [`docs/CHECKPOINTS.md`](./docs/CHECKPOINTS.md) — checked routes and what would change the conclusion.
5. [`evidence/`](./evidence/) — selected sanitized command outputs.

## What would count as real progress

Any of the following would materially change the status:

- public `PRF_R2` or `PRF_R3` samples tied to the target layers;
- `sk.prf_k`, Toeplitz stream material, or reproducible PRF state;
- PC opening material (`value`, `blind`) for target base layers;
- a candidate plaintext with an independent verifier;
- a wallet private key/seed that derives the target address;
- a target-chain state change proving someone else has the key.

Until then, this is a negative-result ledger.

## Publication hygiene

This repo is intentionally sanitized. Do not add:

- GitHub tokens or `github.txt`;
- local wallet files or unrelated secrets;
- unverified private-key claims;
- bulky challenge binaries unless explicitly intended for public redistribution.

