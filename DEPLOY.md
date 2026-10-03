# Deploying this README

Everything here belongs in the repo **`mat-devx/mat-devx`** (default branch `main`), which is
the special profile repo whose README renders at the top of github.com/mat-devx.

## Files to commit

```
README.md
assets/banner.svg
assets/mark.svg
assets/panel.svg
assets/divider.svg
.github/workflows/snake.yml
```

Easiest route without the `gh` CLI — drag-and-drop on GitHub:

1. Go to https://github.com/mat-devx/mat-devx
2. Delete the existing `README.md` (tick "Add a file" → upload, it will ask to overwrite — accept).
3. Upload the four files from `assets/` into an `assets/` folder.
4. Create `.github/workflows/snake.yml`.

Or with git:

```bash
cd mat-devx
cp "C:\Users\MatiCuhcuH\Downloads\mat-devx-profile\README.md" .
cp -r "C:\Users\MatiCuhcuH\Downloads\mat-devx-profile\assets" .
mkdir -p .github\workflows
cp "C:\Users\MatiCuhcuH\Downloads\mat-devx-profile\.github\workflows\snake.yml" .github\workflows\
git add -A && git commit -m "profile: animated AI-engineer README" && git push
```

## Required one-time step: activate the snake

The contribution snake will show a broken image until the workflow has run once.

1. Repo → **Actions** tab → **Generate contribution snake**
2. Click **Run workflow**
3. Wait ~30s, then hard-refresh your profile page (Ctrl+Shift+R).

It then re-runs every 6 hours automatically and commits to the `output` branch.

## Separate, do this yourself

Your profile **bio** still reads "Junior Web Developer" — it renders *above* this README, so
it contradicts the whole page. Change it in **Settings → Profile → Bio** to something like
`AI Engineer · Voice AI · Full-Stack`.

## Notes / gotchas baked into the design

- **`assets/*.svg` are self-contained.** All motion is SMIL inside the SVG, which GitHub plays
  when the file is loaded through `<img>`. GitHub strips `<script>`, inline CSS and `<iframe>`,
  so animation cannot live in the Markdown itself.
- **Every asset's base state is fully legible.** The staggered reveals only *enhance*; if SMIL
  ever fails to run (or a renderer ignores it) you still see the finished design, not a blank.
- **Transform and opacity only.** No `width`/`height`/`x`/`y` animation, and no `feGaussianBlur`
  on large areas — those force layout and continuous repaints.
- **Mono-first type stack.** No webfonts can be loaded from an `<img>`-hosted SVG, so the stacks
  are system monospace fallbacks that resolve everywhere.
- **`divider.svg` uses `gradientUnits="userSpaceOnUse"`** because a horizontal `<line>` has a
  zero-height bounding box, and object-bounding-box gradients never paint there.
- **Third-party services in use** (all verified live): readme-typing-svg, github-readme-stats,
  github-readme-streak-stats, skillicons, shields.io. Nothing else is required — notably
  `github-profile-trophy`, `github-readme-activity-graph`, `github-readme-quotes` and
  `github-skyline` are all currently returning 402/404, so they are deliberately avoided.
- Only external images are the three stats cards; everything visual you own is local.

## Optional follow-up

Your repos have no descriptions (`BDocuLink`, `SmartVote`), so the two project cards land on
blank repo pages. One line each plus a few topics would close that gap:

- **BDocuLink** — "Integrated Barangay Document Management System: document processing, resident
  records and approval workflows. Laravel, React, TypeScript, MySQL." topics: `laravel`, `react`,
  `typescript`, `mysql`, `barangay`, `documents`
- **SmartVote** — "Voting platform covering ballot construction, candidacy and result tabulation."
  topics: `laravel`, `blade`, `mysql`, `voting-system`
