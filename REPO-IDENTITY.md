# REPO-IDENTITY — `presscafe/next-netlify-starter-cns`

**Recorded:** 2026-10-14 · **Corrected:** 2026-10-20 (§2 rows 3, 6, 9 — see §7) · **Record type:** repository passport (audit + reconciliation)
**Governing authority:** COLLECTIVE-OPERATING-CHARTER (ADOPTED 2026-10-09) and PORTFOLIO-REPOSITORY-DISPOSITION-2026-10-02-refresh-2026-10-09
**Status of every disposition below: PROPOSED — nothing executed.** Per the disposition record's rules 7–8 and §12, no archive, delete, detach or merge happens without a recorded principal approval.

---

## 1. Disposition (PROPOSED)

| Field | Value |
| --- | --- |
| **Proposed disposition** | **SUPERSEDED / ARCHIVE-candidate** |
| Confidence | High — every claim below is evidenced against GitHub or this working tree |
| Unique intellectual work present | **None** |
| Blocking dependency for revival | Package-manager repair + TinaCMS credentials (see §5) |
| Requires principal decision | Yes — see §8 approval queue |

Rationale in one line: this is an unmodified upstream template shell plus an auto-generated CMS scaffold, dormant for 2.5 years, with a duplicate sibling in the same account and no evidence of a user, a deployment or a business objective. Under charter §14 (portfolio thinking) it does not earn an active slot; under disposition §12 it is the textbook "abandoned starter shell".

---

## 2. Passport — the twelve checks (disposition §11)

| # | Check | Finding |
| --- | --- | --- |
| 1 | Ownership | `presscafe` (GitHub user id 11492829). A **second** identity of the principal; the authenticated session user is `faroukpandor`. Commit author email `presscafeworld@gmail.com`. |
| 2 | Provenance | Derivative of `netlify-templates/next-netlify-starter` (**MIT**). README still carries the upstream template's own deploy badge and `utm_campaign=nextstarter-cs` links — i.e. upstream boilerplate, never localised. |
| 3 | Fork status | `isFork: false`, `parent: null` — GitHub does **not** track it as a fork, so there is no fork-network link. **Corrected 2026-10-20:** the full upstream commit history *is* intact (109 commits, original authors preserved — `cassidoo`, `renovate[bot]`, `maxcell`, `tzmanics` et al.). This is an *unlinked copy*, not a flattened re-upload. The MIT `LICENSE` file had nonetheless been dropped (restored in this pass — see §7). |
| 4 | Canonical enterprise | None. No capability in the portfolio is canonically hosted here. |
| 5 | Current purpose | Undeclared. `description: ""`, no topics, no homepage, no `REPO-IDENTITY.md` until now. |
| 6 | Last meaningful activity | Created 2022-12-03; **last principal commit 2024-04-01** → dormant ~2.5 years. **Corrected 2026-10-20:** the repo holds **109 commits**, of which **108 are upstream** and exactly **1 is the principal's**. The earlier "one commit in history" reading was an artifact of the audit sandbox holding a shallow, grafted clone (`git rev-parse --is-shallow-repository` → `true`, `.git/shallow` → `3c0f951`). See §7. |
| 7 | Deployment / dependencies | No homepage URL, no Netlify site recorded in-repo. `netlify.toml` still declares the upstream `netlify-plugin-cypress` block. Build requires `NEXT_PUBLIC_TINA_CLIENT_ID` + `TINA_TOKEN` (Tina Cloud), which are not present and not documented. |
| 8 | Secrets / security | **Clean.** No committed credentials; the only token reference is `process.env.TINA_TOKEN` in `tina/config.ts`. `.gitignore` covers `.env*`, `node_modules`, `.netlify`, `tina/__generated__`. |
| 9 | Unique intellectual work | **None.** The principal's single commit (`3c0f951`, 2024-04-01) touches 13 files, +9,804/−2,065 — but ~9,460 of those added lines are machine-generated (`yarn.lock` +8,883, `package-lock.json` +577, `tina-lock.json`, `next-env.d.ts`, `.gitpod.yml`). The human-authored remainder is the TinaCMS CLI scaffold (`tina/config.ts`, 57 lines), a 218-line demo page that labels itself *"a demo file once you have tina setup feel free to delete this file"*, `tsconfig.json`, and 9 lines of lorem ipsum. No product code, no content, no configuration unique to any enterprise. |
| 10 | Duplication | **Yes — direct duplicate.** See §3. |
| 11 | Disposition | Assigned above (PROPOSED). |
| 12 | Decision record | This file. To be mirrored into the portfolio disposition doc once approved. |
| 13 | Execute | **Withheld pending approval.** |

---

## 3. Duplicate family (violates disposition §2 — "one capability, one canonical home")

Two owned repos in the same account implement the same capability ("Next.js + Netlify starter shell"):

| Repo | Size | Last push | Delta |
| --- | --- | --- | --- |
| `presscafe/next-netlify-starter` | 716 KB | 2022-12-03 | Plain upstream template + Cypress |
| `presscafe/next-netlify-starter-cns` (this one) | 890 KB | 2024-04-01 | Same, **plus** the TinaCMS scaffold |

The `-cns` repo strictly supersedes the plain one. Neither carries product work. Neither has a recorded disposition. Recommended handling: **one archive decision covering both**, not two separate debates.

---

## 4. Technical condition (why revival is not free)

Verified in this session, not inferred:

1. **`npm ci` fails on a clean clone.** The committed `package-lock.json` is out of sync with `package.json` — npm reports missing `@esbuild/*` platform binaries, `@udecode/plate-indent`, `@udecode/plate-normalizers`, `@floating-ui/react`, `dependency-graph`, `use-context-selector`. Reproducible install is therefore broken as committed.
2. **Two lockfiles are committed** (`package-lock.json` *and* `yarn.lock`), so the package manager is ambiguous — the direct cause of (1) drifting unnoticed.
3. **The demo page imports a git-ignored, absent module:** `pages/demo/blog/[filename].tsx` imports `tina/__generated__/client`, and `tina/.gitignore` excludes `__generated__`. A clean tree cannot resolve that import until Tina codegen runs.
4. **Stack is end-of-life:** Next `^12.1.5` / React `^18` (upstream is now Next 16 / React 19). `next.config.js` present upstream was dropped here.
5. **TinaCMS pulls toward paid/cloud dependency** (clientId + token), which sits against charter §6 (capital-constrained, free-tier-first) unless run in local mode.
6. **The dependency tree contains a native module.** `tinacms@1.6` pulls in `better-sqlite3`, which must either download a prebuilt binary or compile via `node-gyp` against the Node headers for the installing Node version. That is a real, recurring build-fragility cost on any CI or fresh machine (and a known source of Netlify build failures), independent of this sandbox.

**Verification limit — stated plainly (charter §22):** a full clean-clone build could **not** be confirmed here. The `npm install` attempt failed while compiling `better-sqlite3`: `prebuild-install` could not verify a TLS certificate and `node-gyp` could not reach `nodejs.org` for headers, because this sandbox's network allowlist excludes that host. So `npm run build` was never reached (`tinacms: not found`). Finding 6 is therefore evidenced by the dependency graph, and the *build outcome* is recorded as **UNVERIFIED — environment-limited**, not as a repo defect. Finding 1 is unaffected: npm's lockfile/`package.json` consistency check is local, so the desync is a genuine committed defect.

Consequence: findings 1–2 alone are enough to break a reproducible clean-clone install, and the repair cost exceeds standing up a current template from scratch. That — not the unverified build — is the decisive input for §1.

---

## 5. Cheapest viable path if a CMS-backed static site is ever actually wanted

Do **not** revive this repo. Under charter §6/§7 (free-tier-first, ownership, minimal lock-in) and §14 (reuse before creating):

1. Start from a **current** upstream template (Next 16 / React 19) rather than repairing a 2024 shell.
2. Prefer a **git-backed CMS with no mandatory cloud token** (Decap CMS — already forked in `presscafe/decap-cms` — or Pages CMS, also already forked) over Tina Cloud, keeping content in-repo and cost at zero.
3. Host on **Cloudflare Pages** to match the recorded default stack, rather than adding Netlify as a second platform for the same capability.
4. Delete the demo route and the second lockfile on day one; commit one lockfile only.

---

## 6. Portfolio-scope gap (escalation — larger than this repo)

The 2026-10-09 "Live refresh" audited **`faroukpandor` only** (39 repos: 6 owned, 33 forks). It did not cover `presscafe` at all. Evidence gathered here:

- `presscafe` holds **125 repositories — 35 owned, 90 forks**. That is ~3× the audited account and entirely outside recorded governance.
- **83 of 125** have not been pushed since before 2025-01-01; **6 are empty (0 KB)**.
- Duplicate/entropy clusters already visible in `presscafe`:
  - `hugo-meghna-forestry` + `hugo-meghna-forestry2` + `meghna-hugo` (0 KB)
  - `1-click-hugo-cms` (0 KB) + `one-click-hugo-cms` (53 MB)
  - `js-ecommerce-pwa` + `laravel-ecommerce-pwa` + `rust-ecommerce-pwa` (all 0 KB)
  - `next-netlify-starter` + `next-netlify-starter-cns` (§3)
  - `eleventy-netlify-boilerplate` (fork) + `healthhaven-eleventy-netlify-boilerplate` (owned)
- `healthhaven-eleventy-netlify-boilerplate` falls under disposition §5's **Health family** rule: heightened privacy/security review, and no merge/delete before the intended product/client is identified.
- Large reference forks in `presscafe` mirror the §C2 entropy problem already flagged for `faroukpandor` (e.g. `odoo`, `decap-cms` at ~88 MB, `strapi` at ~642 MB).

This is the higher-leverage finding of the pass: **the portfolio has a second, larger, ungoverned identity.** Repository-count entropy cannot be reduced while one of two accounts is unaudited.

---

## 7. Actions taken in this pass (non-destructive only)

| Action | Authority |
| --- | --- |
| Restored the upstream **MIT `LICENSE`** (verbatim, © netlify-templates) that this derivative had dropped | Disposition §8 — "preserve attribution, licence obligations and provenance"; charter §19 |
| Created this **`REPO-IDENTITY.md`** passport | Disposition §6 rule 3, §11 steps 11–12 |
| Ran a secret scan, duplicate diff, upstream divergence check and an install/build viability test (result partly environment-limited — see §4) | Disposition §11 steps 2, 7–10 |
| **Corrected §2 rows 3, 6 and 9 on 2026-10-20** after GitHub's contributor and commit APIs contradicted the local `git log` reading. The first pass wrongly reported "one flattened commit" and "no fork-network provenance"; the real history is 109 commits with upstream authorship intact. The *conclusion* (no unique intellectual work) is unchanged and now better evidenced. | Charter §22 — distinguish facts from assumptions; §31 — when something fails, diagnose the cause |

**Methodology rule adopted from that error:** for any repository audit, **commit history must be read from the GitHub API, never from a local clone.** A shallow or grafted checkout silently truncates history and produces confident but false evidence. Verify with `git rev-parse --is-shallow-repository` before trusting `git log`. This applies to every future passport in the portfolio audit, not just this repo.

**Not** taken (withheld pending approval): archive, delete, unfork/detach, rename, dependency upgrade, removal of the duplicate sibling, or any change to the GitHub repository settings.

---

## 8. Approval queue — decisions recorded 2026-10-14

| # | Item | Principal decision | Execution status |
| --- | --- | --- | --- |
| 1 | Archive this repo + `presscafe/next-netlify-starter` as one SUPERSEDED family | **APPROVED** | **BLOCKED — not executable by this identity.** `faroukpandor` holds `write` on `next-netlify-starter-cns` and `read` only on `next-netlify-starter`; archiving requires `admin` on both. Remedy: the `presscafe` owner archives them, or grants `admin`, or GitHub is reconnected as `presscafe`. |
| 2 | Extend the passport audit to the `presscafe` account (125 repos) | **APPROVED** | **EXECUTED** — see `PORTFOLIO-AUDIT-PRESSCAFE-2026-10-20`. Read-only; no mutations. |
| 3 | Empty-repo queue in `presscafe` (6 repos) — §6 default flow | Not separately decided | **PROPOSED ONLY** in the portfolio audit. Nothing deleted. |
| 4 | Health repo — confirm intended product/client before anything touches it | Not decided | **ESCALATED, UNCHANGED.** Contributor data strengthens the escalation: `healthhaven-eleventy-netlify-boilerplate` has 261 commits and **zero** by the principal — the "healthhaven" name carries no local work at all. |

Item 1 is recorded as approved-but-blocked rather than done. Per rules 7–8 an approval that cannot be executed must not be reported as executed.

---

## 9. Single most valuable next action (charter §34)

Item 2 was approved and executed, so the next action moves down the queue.

**Grant `admin` on the two `presscafe` starter repos (or have `presscafe` archive them directly), so the approved disposition in item 1 can actually close.**

It is specific, zero-cost, reversible, and it is the only thing standing between a recorded decision and an executed one. Everything else in the portfolio audit is propose-only until the same permission gap is resolved — which makes it, not the audit itself, the real bottleneck. See the portfolio audit's §9 for the larger finding it uncovered: **roughly 15 of 35 owned `presscafe` repos contain zero commits by the principal**, including several whose names imply live Botswana products.
