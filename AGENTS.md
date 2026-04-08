# Parafin docs site — agent instructions

This is the repo for **https://help.parafin.ai** — the Parafin customer-facing help center, tutorials, release notes, and brand reference. Built on [Mintlify](https://mintlify.com).

## Before you edit anything

1. **Read `reference_customer_content_voice.md` in the parent Claude memory** if available — it encodes the docs voice rules.
2. **Read the "Editorial architecture rules" section below** — non-negotiable.
3. **Pull latest** (`git pull origin main`) before starting any work. Mintlify auto-deploys from `main` so assume the remote is ahead.

## Working locally

```bash
cd /tmp/parafin3d-docs    # canonical local checkout
export PATH="/opt/homebrew/opt/node@22/bin:$PATH"   # Mintlify CLI requires Node 22
npx -y mint@latest broken-links    # validate before commit
git add -A && git commit -m "..."  # commit as Adam Hengels <adam@parafin.ai> (config is set in the checkout)
git push origin main    # auto-rebuilds in ~60s
```

If Mintlify doesn't rebuild within 2 min, open the Mintlify dashboard → Manual Update.

## Repo structure

- `docs.json` — navigation config (7-group "Help center" tab)
- `index.mdx` — help center landing
- `getting-started/` — account setup, my-projects, editing, password
- `launching-a-project/` — project creation flow (6 steps + overview)
- `working-with-projects/` — assumptions, filtering/sorting, viewers, exports, executive summary, floor plans, brand contact
- `tutorials/` — deep walkthroughs (currently: `zoning-panel.mdx`)
- `brands/` — customer-facing brand registry (scaffolded; Phase 3 dedicated session expands it)
- `faqs.mdx` — single-page FAQ
- `changelog.mdx` — version-numbered `<Update>` blocks (1.57–1.62 preview)
- `images/releases/<version>/` — changelog-sourced assets, ALSO referenced from help pages
- `images/support/legacy/` — stale Phase 1 placeholder screenshots, to be replaced by Phase 2 Playwright

## Editorial architecture rules (locked 2026-04-08)

These are non-negotiable.

### 1. Help pages are the source of truth. Changelog is a record of changes.

Help pages must be thorough enough that customers never need to click into the changelog for feature details. If a feature shipped in a release and is documented in the changelog with screenshots, that content — **including the images** — belongs on the relevant help page. Don't write "see the 1.59 release notes for details." **Absorb** the content.

**Corollary — zero changelog referrals in help:** grep help pages for `/changelog`, "release notes", version numbers like "1.59". These should NOT appear as referrals pointing readers back to the changelog. They're fine only as in-context anchors stating a fact.

### 2. Don't leave stale legacy images next to new images that replaced them.

If a changelog-sourced image shows a feature in its current state, delete any legacy screenshot of the same feature. Stale + current side-by-side is misleading.

**Exception:** if the legacy image shows content the new images don't cover (input panel field layouts, output tables, dashboards), keep it as a Phase 2 Playwright replacement target.

### 3. Changelog images and help page images share files.

Both the changelog and help pages reference assets under `images/releases/<version>/...`. **Do not duplicate image files** when absorbing changelog content into help pages. Reference the same path from both.

### 4. Docs voice

- **Present tense** for capabilities: "The Zoning Panel shows..." not "We've added..."
- **Imperative** for instructions: "Open the panel..." not "You can open..."
- **Second person "you"**, never "users" or "the user"
- **"We" is rare** — only for team statements like "We recommend..."
- **One idea per sentence.**
- **No hype:** banned list includes `seamless`, `intuitive`, `powerful`, `revolutionary`, `cutting-edge`, `10x`, `delightful`, `effortless`, `instantly`, `elevated`, `streamlined` (as adjective), `unlock`
- **No marketing connectives:** "One of the most...", "By popular demand...", "We're excited to announce..."
- **Benefit first, mechanism second.**
- **1–3 sentences per paragraph.** Bullets for parallel items.
- **Bold for UI elements** (`**Settings**`), code formatting for files/paths/commands.
- **Sentence case for headings** (unless it's a proper product name).

### 5. Never port personal contact info.

The legacy parafin.ai/support page had a personal phone number baked in. It was removed 2026-04-08. Contact is email-only: `support@parafin.ai`.

## When writing a new help page

1. Create `<group>/<slug>.mdx` with `title` + `description` frontmatter
2. Add to `docs.json` navigation under the appropriate group
3. Write in docs voice (rule 4)
4. **Cross-check the changelog** for any shipped feature that touches the page's subject — pull in the content + images (rule 1)
5. Validate: `npx -y mint@latest broken-links`
6. Commit as Adam Hengels, push

## When editing the changelog

Each release is a `<Update label="X.YY" description="Month Day, Year" tags={[...]}>` block. Reverse chronological (newest at top). Use `<Frame caption="...">` for images. Tags stay inside the existing vocabulary: `feature`, `improvement`, `fix`, `zoning`, `editor`, `pcf`, `coming soon`, `variants`.

**Release-workflow context:** changelog and HubSpot release email are co-deliverables shipped the same day. See `project_release_workflow_roadmap.md` in Claude memory. The email voice differs from docs voice — email is narrative, docs is scannable. Keep them distinct.

## Known gotchas

- **Node 22 required** for Mintlify CLI. Node 25+ unsupported.
- **GitHub webhook stalls** occasionally. Use Manual Update in the dashboard.
- **`essentials/*`, `api-reference/*`, `ai-tools/*`, `snippets/*`** are Mintlify starter template pages. Not in nav, not on live site, but 3 pre-existing broken links live there. Ignore unless doing a dedicated cleanup pass.
- **Claude Code Edit tool requires a Read after `git mv`.** If you rename a file and then try to Edit the new path without Reading it first, the Edit silently no-ops.
- **Tool-output artifacts** — rarely `</content></invoke>` has leaked at EOF on very long Writes. Sanity-check `tail -3` after big file writes.

## Live URLs

- **Production:** https://help.parafin.ai
- **Mintlify preview (still works):** https://parafin-16645fe6.mintlify.app
- **Dashboard:** https://dashboard.mintlify.com (root — don't trust any cached deep-link URL)
