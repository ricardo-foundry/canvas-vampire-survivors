# Post-Release Verification — Round 25

_Date: 2026-04-25_

Single-pass post-release verification across the three projects shipped
in Round 24:

- **canvas-vampire-survivors v2.8.0**
- **openhand v0.8.0**
- **terminal-quest-cli v2.8.0**

All checks below were performed against the public, deployed artifacts
(GitHub Pages / GitHub Releases / npm pack).

---

## 1. canvas-vampire-survivors v2.8.0

| Check                                                                 | Expected | Actual | Result |
|-----------------------------------------------------------------------|----------|--------|--------|
| `GET https://ricardo-foundry.github.io/canvas-vampire-survivors/`     | 200      | 200    | PASS   |
| `GET .../service-worker.js` (iter-12 fix — must not 404)              | 200      | 200    | PASS   |
| `GET .../manifest.json`                                               | 200      | 200    | PASS   |
| `index.html` `og:title` contains "Survivor"                           | yes      | `Survivor — Open Source Roguelite` | PASS |
| GitHub Release `v2.8.0` exists, not draft, not prerelease             | yes      | published 2026-04-25T09:45:01Z | PASS |

Notes:

- Service worker is back to **200**, confirming the iter-12 PWA fix is
  live in production. No regression.
- Release notes title: "v2.8.0 — Onboarding, Replays & Polish".

**Section result: PASS (5 / 5).**

---

## 2. openhand v0.8.0

| Check                                                            | Expected         | Actual                           | Result |
|------------------------------------------------------------------|------------------|----------------------------------|--------|
| `GET https://ricardo-foundry.github.io/openhand/`                | 200              | 200                              | PASS   |
| `GET .../build-meta.json`                                        | 200, fresh data  | 200, schema=1                    | PASS   |
| `build-meta.json` reflects v0.8.0 commit                         | release commit   | `34f0434 release(iter-24): v0.8.0` | PASS |
| `build-meta.json` `audit.vulnerabilities`                        | 0                | 0                                | PASS   |
| `build-meta.json` `tests.total`                                  | non-zero         | 376 (231 unit + 18 e2e + 35 integ + 82 plugins + 10 bench) | PASS |
| GitHub Release `v0.8.0` exists, not draft, not prerelease        | yes              | published 2026-04-25T09:46:51Z   | PASS   |

Notes:

- `build-meta.json` `generatedAt` = 2026-04-25T09:48:09Z, well after
  the release commit — Pages rebuild definitely picked up v0.8.0.
- Release title: "v0.8.0 — MCP, Sandbox v2, fortune-cookie, chaos suite".

**Section result: PASS (6 / 6).**

---

## 3. terminal-quest-cli v2.8.0

| Check                                       | Expected     | Actual    | Result |
|---------------------------------------------|--------------|-----------|--------|
| `npm pack --dry-run` package size           | < 200 kB     | 196.9 kB  | PASS   |
| `npm pack --dry-run` version                | 2.8.0        | 2.8.0     | PASS   |
| `npm pack --dry-run` total files            | reasonable   | 82        | PASS   |
| GitHub Release `v2.8.0` exists, not draft   | yes          | published 2026-04-25T09:44:42Z | PASS |

Notes:

- Package size lands at **196.9 kB** — under the 200 kB budget but
  with only ~3 kB of headroom. Worth keeping an eye on next release.
- Unpacked size is 619.5 kB across 82 files; nothing obviously
  shippable was forgotten (quests, src/, i18n all present).
- Release title: "Terminal Quest v2.8.0".

**Section result: PASS (4 / 4).**

---

## Summary

| Project                       | Version | Result      |
|-------------------------------|---------|-------------|
| canvas-vampire-survivors      | v2.8.0  | PASS (5/5)  |
| openhand                      | v0.8.0  | PASS (6/6)  |
| terminal-quest-cli            | v2.8.0  | PASS (4/4)  |

**Overall: PASS (15 / 15). No failures, no waiting-on-Pages, no
regressions.**

All three releases are live and serving the expected v2.8.0 / v0.8.0
artifacts. Round 24 shipped cleanly.
