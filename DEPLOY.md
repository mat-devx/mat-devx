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
```

This folder is already a git repo on branch `main` with `origin` set to
`https://github.com/mat-devx/mat-devx.git`. To publish:

```powershell
cd C:\Users\MatiCuhcuH\Downloads\mat-devx-profile
git add -A
git commit -m "profile: update README"
git push -f origin main
```

Force-push because the remote repo has its own unrelated commit history and the
README was replaced wholesale.

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
- **Third-party services in use:** readme-typing-svg, skillicons, shields.io. All verified
  live. Nothing else is required — the contribution snake, `github-readme-stats` and
  `github-readme-streak-stats` cards were all removed, so there are no remaining
  dependencies on those services (several are unmaintained and return 402/404 anyway).
- Every image on the page is either a local asset or one of the three services above.

## Optional follow-up

Your repos have no descriptions (`BDocuLink`, `SmartVote`), so the two project cards land on
blank repo pages. One line each plus a few topics would close that gap:

- **BDocuLink** — "Integrated Barangay Document Management System: document processing, resident
  records and approval workflows. Laravel, React, TypeScript, MySQL." topics: `laravel`, `react`,
  `typescript`, `mysql`, `barangay`, `documents`
- **SmartVote** — "Voting platform covering ballot construction, candidacy and result tabulation."
  topics: `laravel`, `blade`, `mysql`, `voting-system`
