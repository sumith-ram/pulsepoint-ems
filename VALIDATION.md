# Validation and dependency disposition

Assessment date: 2026-10-16. Dependency versions and saved forecast/policy metrics were not changed. A pre-existing test-output side effect and its isolation fix are described below.

## Fresh isolated dependency assessment

Only `package.json` and `package-lock.json` were copied to a private assessment directory. `npm audit --json` completed against the registry with exit 1: **2 critical and 1 moderate affected packages**, matching the saved concern. `npm audit --omit=dev --json` completed with exit 0 and zero findings. Registry/advisory information is a dated observation, not a future guarantee.

| Package | Locked scope | Fresh applicability / upstream disposition | Exposure |
| --- | --- | --- | --- |
| `@vitest/mocker` 3.2.7 | Transitive dev/test | Moderate; locked range still flagged. Audit recommends Vitest 5.0.5, a major migration from 3.x. | Mock/redirect test tooling; not shipped as application runtime code |
| `tinypool` 1.1.1 | Transitive dev/test | Critical; locked range still flagged. Upstream fixed lines exist but replacing the current major transitively is not a verified nonbreaking fix. | Test worker execution with attacker-controlled/malicious options; not an EMS runtime feature |
| `vitest` 3.2.7 | Direct dev/test | Critical aggregate finding including transitive packages; no verified safe 3.x fix offered by this audit. | Test/build environment; avoid treating dev/test tooling as a public production service |

Official advisory evidence: [mocker redirect issue](https://github.com/advisories/GHSA-82fw-gwwq-j7x9), [tinypool worker-options issue](https://github.com/advisories/GHSA-5gmw-xhrv-c9v3), [tinypool run-options issue](https://github.com/advisories/GHSA-85c8-ppgw-ccpr).

Classification: **still applicable in the assessed lockfile; development/build/test-only exposure; upstream fixes require migration review**. Production-only audit is clear for its registry scope at assessment time, but that does not clear the development environment or guarantee runtime safety. No breaking major upgrade, forced fix or unverified dependency override was applied. No application source is distributed in this export, so the vulnerable tooling is not a dependency of the case-study files.

## Current validation

- Existing Python suite: **46 passed**, exit 0; cache/bytecode generation disabled for the run.
- Existing frontend suite: **6 passed**, exit 0; includes retrospective/limited-reliability text, Staten Island review behavior, all scenario/cost combinations and theme/data contracts.
- TypeScript `--noEmit`: PASS, exit 0.
- Vite product build: PASS, exit 0; output went to a private isolated validation directory, preserving the historical release product.
- Build output: approximately 281.47 KB JS (81.49 KB gzip), 10.79 KB CSS (2.86 KB gzip); local build evidence, not serving latency.

## Existing test-output side effect and bounded correction

The first Python run exposed an existing lag-feature unit test that called a persistence-writing function without redirecting its output directories. It regenerated `data/curated/phase_4/demand_features_v1/demand_features_v1.parquet` and `experiments/phase_4/feature_table.json` from its eight-row synthetic fixture. The current metadata now records eight fixture rows. Pre-run bytes were not backed up, so exact restoration of those two artifacts is not claimed; unchanged file sizes are not proof of unchanged contents. This was a validation side effect, not a change made to improve publication claims.

The existing test was corrected to monkeypatch its project, curated-output and experiment directories into pytest temporary storage. No analytical implementation or expected feature behavior changed. The full Python suite was then rerun: **46 passed**. Before/after SHA-256 and modification-time guards prove that the two canonical output files were untouched by the corrected rerun. Full snapshot reconstruction should verify/regenerate those feature outputs from approved analytical inputs before claiming end-to-end reproduction. No raw-pipeline rebuild or invented historical timestamp was used to hide the side effect.

No data reacquisition, full raw-pipeline rebuild or prospective operational evaluation was attempted. Saved forecast/policy/interval result files were not regenerated. The dependency assessment provides no reason to remove LIMITED_RELIABILITY, false-alarm/precision/abstention context, surge undercoverage or Staten Island REVIEW_REQUIRED.

## Remaining review

Owner source/asset publication decisions, provider/privacy/naming review and a separately justified Vitest migration if source/test tooling is later distributed. A runnable full-source release remains REVIEW_REQUIRED even though this documentation-only export excludes the tooling.
