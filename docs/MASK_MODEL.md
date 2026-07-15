# Mask model checkpoint

This note is intentionally short. It records the mask dependency that drives the current blocked conclusion.

## Public side

For a base layer, edge aggregation gives a public field numerator. That numerator is already multiplied by a hidden mask.

For wrapped text, two base layers are combined with a fresh random wrapper mask. Without the hidden layer masks, the public numerators do not score plaintext guesses.

## Hidden side

The relevant implementation path computes the layer mask from three domain-separated PRF components:

```text
PRF_R1 = prf_R_core(..., "pvac.prf.r.1")
PRF_R2 = prf_R_core(..., "pvac.prf.r.2")
PRF_R3 = prf_R_core(..., "pvac.prf.r.3")
R      = PRF_R1 * PRF_R2 * PRF_R3
```

The PRF core is tied to `sk.prf_k`, public key digests/tags, layer seed/nonce, and Toeplitz-derived extraction.

## Consequence

A route that only touches `pvac.prf.r.1` is incomplete. It may help characterize the side sample release, but by itself it does not produce a plaintext oracle.

## Useful falsifiers

The blocked conclusion would change if a public lead exposes one of these:

- `pvac.prf.r.2` / `pvac.prf.r.3` samples for target layers;
- the PRF key or a stream equivalent;
- repeated layer masks or nonces;
- PC openings;
- a public verifier for plaintext guesses.
