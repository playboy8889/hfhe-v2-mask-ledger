# Reproduction notes

This publication copy stores selected outputs under `evidence/`. Full local reproduction expects the private working directory layout at `/Users/koala/hfhe_challenge_v2` and the upstream challenge artifacts.

Typical local checks used for the ledger:

```bash
python3 hfhe_monitor_once.py
python3 twitter_public_search_refresh.py
git ls-remote --heads --tags https://github.com/octra-labs/hfhe-challenge.git
```

Specialized C++/Python probes are recorded by timestamp in `evidence/`, for example:

- bundle boundary audit
- seed/PC collision audit
- public key matrix audit
- PRF domain dependency audit
- external lead triage

Candidate secrets are not published unless locally validated.
