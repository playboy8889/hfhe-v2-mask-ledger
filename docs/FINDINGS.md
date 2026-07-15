# Findings snapshot

## Still missing

- target plaintext
- target wallet private key
- `PRF_R2`
- `PRF_R3`
- `sk.prf_k`
- Toeplitz stream equivalent
- PC openings

## Informational observations

- Plaintext length is constrained by bundle count/framing, but this is not a verifier.
- Public side samples are one-domain (`pvac.prf.r.1`) evidence, not full-mask evidence.
- Public PC points are hiding commitments under unknown blindings.
- Public H/UBK structure has not shown a collapse useful for recovery.

## Current operational rule

Do not treat social “solved” claims as progress unless they include a locally checkable artifact or a target-address state change.
