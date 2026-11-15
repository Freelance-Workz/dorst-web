# Next session — Dorst Web

**Path:** `/home/ikigai/Dev/clients/dorst/dorst-web`  
**Engagement:** `freelance-gig` → **framework** `forge`  
**Venture:** `_inputs/venture.json` → Forge `proj03-dorst-web`

---

## Session — 10 Nov 2026 (partner portal QA: password notice, stock check)

**Chat scope:** this thread (+ forks), not whole-repo  
**Branch (this chat):** `fix/partner-portal-qa-2026-11-10` @ `d5230a8`, merged to `staging` via integration PR [#22](https://github.com/Rumi-Shamz/dorst-web/pull/22) (`9c5c707`); local branch removed  
**Related:** ERP PR #164 (invoice PDF, B2C confirmation), ERP issues #168, #169  
**Engagement:** `freelance-gig` → `forge`  
**Framework log:** `projects-feedback/proj03-dorst-web/logs/sessions/2026-11-10-partner-portal-qa.md`

### What happened this session
- Staging shop showed no captcha: the Preview environment had no Turnstile site key. Preview now has Cloudflare's always-pass test key (config only, no code change).
- Partner login: the "password sent" line is now a green status banner naming the address (`846f2c2`).
- Partner order: the stock check could finish after the partner reached payment and zero an out-of-stock line, so Confirm posted no lines and the ERP answered "Invalid input". "Continue to payment" now awaits a stock check for every line, stays on the order step when nothing is left, and submit refuses an empty order (`d5230a8`).
- `next build` passes. No ESLint in this repo, so no lint ran.

### Commits this session
| Commit | Summary |
|--------|---------|
| `846f2c2` | fix(partner): make the 'password sent' notice prominent |
| `d5230a8` | fix(partner): check stock before payment so an order can't go out empty |
| `9c5c707` | Merge #22 (integration/2026-11-10) into `staging` |

### Next actions
#### Must do
- [ ] Prod smoke: B2C checkout confirmation email ([#11](https://github.com/Rumi-Shamz/dorst-web/issues/11)); the ERP now sends it immediately (ERP #168 needs the owner's OK first)
#### Should do
- [ ] Add ESLint (`npm run lint` fails: `eslint` is not installed)
#### Nice to have
- [ ] Real Turnstile widget on staging, if wanted (hostname must be in the Cloudflare allowlist)

### Human verification
- [ ] Staging shop: the captcha widget shows and an order goes through
- [ ] Staging partner portal: resend password shows the green banner; an out-of-stock line is removed before payment
## Session — 10 Nov 2026 (checkout name field, privacy policy; release to staging #22)

**Chat scope:** this thread (+ forks), not whole-repo
**Branch (this chat):** none left open (`integration/2026-11-10` merged as `9c5c707` and deleted; this handoff on `chore/close-session-2026-11-10-checkout-privacy`)
**Related:** #22; issues #23, #24; ERP PR #164 (companion)
**Engagement:** `freelance-gig` → `forge`
**Framework log:** `projects-feedback/proj03-dorst-web/logs/sessions/2026-11-10-checkout-name-privacy.md`

### What happened this session
- **Checkout asks for the buyer's name** (required, EN/BG) and sends it as `customer_name`. The ERP (#164) files each shop order under a private customer record named after the buyer, so it can be invoiced; a returning email reuses that record.
- **Privacy policy:** there was none; the footer links pointed to `#`. Added `/privacy` (EN/BG), written from what the code collects and which services it uses (Vercel incl. Analytics, Supabase, Resend, Cloudflare Turnstile, Upstash), cookies and browser storage, retention and rights. Reachable without the age gate. Privacy and Cookie Policy footer links open it; Terms of Sale still points to `#` (#24). Wording needs operator review before production (#23).
- **Shipped together** with the other thread's partner portal fixes (prominent "password sent" notice, stock check before payment) in one integration PR, #22. `staging.dorst.top/privacy/` verified 200 in both languages after the deploy.
- **Tooling notes:** worktree builds need a real `pnpm install` (Turbopack rejects a symlinked `node_modules`); `pnpm install` may create a `pnpm-workspace.yaml` asking about build scripts, don't commit it. The repo has no `eslint.config.*`, so `eslint` can't run.

### Commits this session
| Commit | Summary |
|---|---|
| `453788c` | feat(shop): ask for the buyer's name at checkout |
| `c7a55b7` | feat(legal): add a privacy policy page (EN/BG) and link it from the footer |
| `9c5c707` | #22 integration/2026-11-10 → staging (with `846f2c2`, `d5230a8` from the partner portal thread) |

### Next actions
#### Must do
- [ ] Review the privacy policy wording before production (#23)
- [ ] Test a shop order with a name on staging, then release staging → main together with ERP #164
#### Should do
- [ ] Terms of Sale page (#24)
#### Nice to have
- [ ] Add an ESLint flat config so `pnpm lint` works

### Human verification
- [ ] `staging.dorst.top/shop/checkout/` shows the Full name field; `/privacy/` reads correctly in EN and BG

---

## Session — 11 Sep 2026 (cleanup + export fix)

**Branch at wrap-up:** `main` @ `4b17406`  
**Framework log:** `projects-feedback/proj03-dorst-web/logs/sessions/2026-09-11-web-cleanup-export-fix-close.md`

### What happened
- Cleanup to main (`17a5347`): no Sanity/Stripe/ui dump; brand-book kept under `public/brand-book/`.
- Dev fix (`4b17406`): export only for GitHub Pages; turbopack.root; client age-gate fallback.

### Next actions
#### Must do
- [ ] Partner portal brand-book download when logged in ([#8](https://github.com/Rumi-Shamz/dorst-web/issues/8))
#### Should do
- [ ] Commit `_inputs/venture.json` on main
- [ ] Drop obsolete remotes if unused
#### Nice to have
- [ ] Whale jump animation (TODO in WhaleHero)

### Human verification
- [ ] `npm run dev` on :3000 after killing orphan next processes; age gate redirects
