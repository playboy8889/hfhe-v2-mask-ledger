# Checkpoints

This file lists the active audit checkpoints in a different format from the earlier public mirror.

## A. Artifact container

- Bundle magic and length framing parse cleanly.
- 22 ciphertext entries were observed.
- No trailing hidden PVAC/OCTRA block was found.
- No long printable ASCII pocket was found.

## B. Layer randomness

- 44 base-layer seeds were parsed.
- No full seed reuse was observed.
- No zero seed component pattern was observed.
- Minimum nonce Hamming distance did not suggest near-reuse.

## C. Commitment surface

- 44 PC points were observed.
- They are serialized as Ristretto points, not openings.
- No PC point reuse or all-zero PC point was observed.
- `ComOpen.value` and `ComOpen.blind` remain local witness material, absent from `secret.ct`.

## D. Public key structure

- H columns are nonzero and nonduplicate in the latest audit.
- H rank is full over the row space.
- UBK permutation/inverse arrays are consistent.

## E. External leads

- Kubo/smoke-ui Day 6 algebraic-track post was imported as a negative baseline.
- The linked repository reports no public-only plaintext recovery.
- No target secret, plaintext, PRF key, or PC opening was found in that triage.

## F. Next trigger

Escalate only if a lead contains a concrete artifact or candidate that can be checked locally.
