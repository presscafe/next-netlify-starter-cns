# REPO-IDENTITY — `presscafe/next-netlify-starter-cns`

**Recorded:** 2026-10-14 · **Record type:** repository passport (audit + reconciliation)
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
| 3 | Fork status | `isFork: false`, `parent: null`. Not a GitHub fork, so **no fork-network provenance** — history was flattened into a single commit titled `commit` (2024-04-01). Attribution survived only via README, and the MIT `LICENSE` file had been dropped (restored in this pass — see §7). |
| 4 | Canonical enterprise | None. No capability in the portfolio is canonically hosted here. |
| 5 | Current purpose | Undeclared. `description: ""`, no topics, no homepage, no `REPO-IDENTITY.md` until now. |
| 6 | Last meaningful activity | Created 2022-12-03; **last push 2024-04-01** → dormant ~2.5 years as at 2026-10-14. Exactly one commit in history. |
| 7 | Deployment / dependencies | No homepage URL, no Netlify site recorded in-repo. `netlify.toml` still declares the upstream `netlify-plugin-cypress` block. Build requires `NEXT_PUBLIC_TINA_CLIENT_ID` + `TINA_TOKEN` (Tina Cloud), which are not present and not documented. |
| 8 | Secrets / security | **Clean.** No committed credentials; the only token reference is `process.env.TINA_TOKEN` in `tina/config.ts`. `.gitignore` covers `.env*`, `node_modules`, `.netlify`, `tina/__generated__`. |
| 9 | Unique intellectual work | **None.** Diff vs upstream shows only: TinaCMS CLI scaffold (`tina/config.ts`, `tina/tina-lock.json`, `content/posts/hello-world.md` lorem ipsum, and `pages/demo/blog/[filename].tsx` — a file that labels itself *"This is a demo file once you have tina setup feel free to delete this file"*), plus Cypress config and TS config. All generated or boilerplate. |
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

**Not** taken (withheld pending approval): archive, delete, unfork/detach, rename, dependency upgrade, removal of the duplicate sibling, or any change to the GitHub repository settings.

---

## 8. Approval queue (one decision per item)

1. **This repo + `presscafe/next-netlify-starter`** — approve ARCHIVE for both as a single SUPERSEDED family? (Recommended: yes. No unique work is lost; history remains readable in an archived repo.)
2. **Scope extension** — approve extending the repository-passport audit to the `presscafe` account (125 repos), using the same twelve checks and the same propose-don't-execute rule? (Recommended: yes; highest leverage.)
3. **Empty-repo queue in `presscafe`** (6 repos) — approve the §6 default flow (confirm intent → delete, or mark EXPERIMENT with a `REPO-IDENTITY.md`, or archive)?
4. **Health repo** — confirm the intended product/client for `healthhaven-eleventy-netlify-boilerplate` before anything touches it.

---

## 9. Single most valuable next action (charter §34)

**Approve or reject item 2 — extend the audit to `presscafe`.**

It is specific, zero-cost, reversible, and it is the only action that changes the portfolio's trajectory: the 2026-10-09 record cannot deliver "one canonical home per capability" or "zero unexplained empty repositories" while 125 repos in a second identity remain unexamined. Archiving this particular shell (item 1) is tidy but marginal by comparison.
