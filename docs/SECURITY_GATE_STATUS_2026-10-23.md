# whispr-gate security verification checkpoint — 2026-10-23

**Canonical approved project:** QMOOSA-0670, Batch 134, tests component of the encrypted-chat family. Actual repository: `elon00/whispr-gate`, GitHub repository ID 1426437824, owner `elon00`, not a fork.

## Evidence and limitations
- [Prior main-HEAD run 39721270147](https://github.com/elon00/whispr-gate/actions/runs/39721270147) completed **failure** before test execution.
- The old workflow used invalid `actions/checkout@4` and `github/codeql-action/*@3` action refs; its only "test" printed `100/100 VERIFIED` without executing a regression suite.
- Both existing `tests/reality_battery.py` and `tests/reality_check_harness.py` contain malformed binary-like text, including control characters. They are retained as evidence and must not be replaced with trivial passing assertions.
- No independently verified encrypted-chat protocol implementation, threat model, authenticated key exchange, message replay protection, or end-to-end encryption interoperability test was found in this **tests-only** repository. This is not proof that the entire family lacks an implementation; inspect the protocol/engine repos separately.
- Architecture documentation previously claimed production and `100/100 VERIFIED` without corresponding test evidence. These claims are corrected on this review branch.

## Changes proposed on this branch
1. Replace invalid action refs with `actions/checkout@v4`, `github/codeql-action/*@v3`; analyze the actual Python language.
2. Require `python -m compileall -q tests` and `python -m pytest -q tests` as separate **fail-closed** checks. A red job is expected until corrupted test sources are properly repaired.
3. Explicitly mark architecture capabilities as proposed, not implemented or certified.

## Acceptance blockers
- Recover or rewrite the corrupted tests from reviewed source, tied to real encrypted-chat protocol/engine code (not mocks). Required negative tests: unauthenticated messages, wrong-key decrypt, tampered ciphertext/tag, nonce reuse, replay/out-of-order messages, key rotation, revoked devices, and crash recovery where applicable.
- Confirm authenticated encryption and key exchange use audited libraries; avoid new custom cryptography.
- Run complete tests and CodeQL on the PR HEAD, fix findings and obtain independent security review.
- Keep this PR **draft** and `SECURITY_APPROVED` / `DEPLOYMENT_READY` **false** until independently evidenced.

No deployment, credentials, paid operations, gas/cycles, force-push, FIPS claim, or security-gate bypass was performed.
