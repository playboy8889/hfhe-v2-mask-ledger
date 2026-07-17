# HFHE v2 public-method study 20260718

## Scope
Reviewed public GitHub repositories cloned under `/Users/koala/hfhe_challenge_v2/method_study_20260718`:
- startkey public audit
- asbryx source-first cryptanalysis
- smoke-ui security assessment
- Kubo public solver/audit
- Chimdalu-Ofoegbu decryptor attempt
- lustrousys public audit
- lamafab Lean formalization

No repository in this review published active-target plaintext, target wallet private key, `sk.bin`, `plaintext.txt`, `PRF_R2`, `PRF_R3`, `sk.prf_k`, Toeplitz secret stream, PC openings/blinds, or a reusable candidate-check oracle.

## Method families learned

### 1. Artifact-structure first
Common pattern: parse `pk.bin` and `secret.ct`, confirm hashes/counts, enumerate layers/edges/nonces/seeds, then separate metadata leakage from semantic plaintext leakage.
Useful locally: keep our parser/serializer checks as a gate before any claimed candidate. Any future candidate must round-trip against the active artifact, not just fit a toy schema.

### 2. V1-oracle vs v2-no-oracle distinction
Public audits emphasize that v1-style public commitment verification does not carry over to v2. V2 public data does not expose a cheap predicate for guessed plaintext blocks.
Useful locally: reject routes that only prove parser acceptance or guessed formatting; require a target-bound verifier or independent key material.

### 3. R1 LPN corpus analysis
asbryx/smoke-style work treats the July 11 corpus as exact public R1 data: helpful for validating candidate `S` or PRF hypotheses, but not sufficient to generate the missing full mask. The recurring dependency graph is:
`published A_R1/y_R1 -> verifier for candidate S/prf path`, but plaintext still needs R2/R3/Toeplitz/PRF material or another bridge.
Useful locally: use R1 as a filter if a candidate `prf_k`/`S` appears; do not spend cycles claiming R1 alone decrypts.

### 4. PRF/Toeplitz/domain audits
Several repos test PRF domains, Toeplitz rank/window behavior, counter layout, SHA/AES transcript diffusion, and truncation assumptions. Negative result: no public rank collapse or domain collision that yields `R`.
Useful locally: next high-value technical check is not generic Toeplitz rank again; it is looking for implementation-version drift, reused PRF inputs, or leaked generation logs.

### 5. Ciphertext algebra and subgroup projections
smoke-style algebra abstracts the two BASE layers as masked field equations. With independent unknown `R0/R1` and wrapper mask `m`, equality/projection tests do not identify plaintext.
Useful locally: only reopen algebra if a new public relation binds R0/R1/R2/R3 across layers, or if an implementation bug forces reuse/correlation.

### 6. Known-plaintext / wallet-template constraints
Multiple attempts model plausible plaintext format and wallet/KDF candidates. Negative result: even very strong template constraints leave too many field degrees without a mask/key bridge.
Useful locally: keep mnemonic/private-key dictionary scans as low-priority unless tied to a leaked clue, repo commit, or X post.

### 7. Implementation and parser hardening probes
Kubo/smoke-style malformed-structure and parser probes look for accepted malformed ciphertexts or serialization quirks. Negative result so far: no active plaintext recovery path.
Useful locally: if we continue technical work, target source commit drift and exact producer/decrypt path rather than fuzzing random parse variants.

### 8. Formalization / Lean route
lamafab formalizes high-level logic/hypergraph concepts; useful for reasoning, but it does not contain target recovery material.
Useful locally: use it only to sanity-check algebraic assumptions, not as a direct solver.

## New local operating rule
Treat public repos as filters and falsification libraries, not as solved templates. Reuse their strongest gates:
1. Verify artifact identity and parser round-trip.
2. Map any proposed route onto missing-material graph: plaintext requires complete `R` or equivalent oracle.
3. If a candidate `S`/`prf_k` appears, test it against public R1 corpus first.
4. If a candidate plaintext/private key appears, verify against `secret.ct`, `pk.bin`, and target wallet before any report.

## Practical next routes
- Monitor for generation-side leaks: `plaintext.txt`, `sk.bin`, `sk.prf_k`, `lpn_s_bits`, `PRF_R2`, `PRF_R3`, producer logs, or forks with active artifact variants.
- Compare public repos for new source-code deltas, not just README claims.
- Build/keep a lightweight R1 candidate verifier locally so a future candidate `S` or `prf_k` can be rejected quickly.
- Avoid re-running closed broad routes: generic ciphertext algebra, Toeplitz rank, parser acceptance, and wallet-template guessing without new evidence.
