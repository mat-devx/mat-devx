# Deploying this profile

This folder is the source for the repo **`mat-devx/mat-devx`** (default branch `main`) — the
special repository whose `README.md` renders at the top of github.com/mat-devx.

## Files

```
README.md
assets/banner.svg     hero — terminal window: prompt, answer, tool call, status bar
assets/mark.svg       MV identity card — prompt line, monogram, handle, availability
assets/panel.svg      profile panel — tool call, 7 child rows, status footer
assets/divider.svg    prompt-chevron section rule
```

All four `assets/*.svg` are referenced by the README through **relative paths**, so they must
sit next to `README.md` in the repo. They already exist on the remote.

## Publish

The remote diverged: `origin/main` and your local `main` both contain edits made after the same
commit (`a9b59c5`), so a normal `git push` will be rejected. Publish the whole design in one go:

```powershell
cd C:\Users\MatiCuhcuH\Downloads\mat-devx-profile
git add -A
git commit -m "profile: agentic CLI redesign"
git push --force-with-lease origin main
```

`--force-with-lease` overwrites `origin/main` **only if** it still points where you last saw it —
safer than `--force`. If it refuses, `git fetch origin` then retry.

### No `gh` CLI / prefer the browser

1. https://github.com/mat-devx/mat-devx → **Add file → Upload files**
2. Drag `README.md` **and the whole `assets/` folder** in; confirm overwrite.
3. Commit.

> Upload all four SVGs, not just `README.md`. The README only links to files, so any asset you
> skip keeps showing the previous design.

## One-time, do this in GitHub settings

Your profile **bio** still reads `Junior Web Developer`. It renders **above** this README and
contradicts the whole page. Settings → Profile → Bio → e.g. `AI Engineer · Voice AI · Full-Stack`.

## Design notes / gotchas

### The metaphor: one screen of an agentic CLI session

The profile is styled as a Hermes-style terminal transcript. Nothing here is decoration — each
symbol maps to a real meaning:

| symbol | in the CLI | here |
| --- | --- | --- |
| `❯` | input prompt | the line that "asks" who you are (banner, mark) |
| `●` | a tool call | `Read profile.yaml`, `Read about.md`, `search.web` |
| `┊` | tool-activity rail | dashed rail bracketing a call's children |
| `└─` | tree rail | the single child entry under a tool call |
| `✓` | tool result | `84ms` / `3 refs`, right-aligned on the tool line |
| `▾` | expanded section chevron | the `profile` panel title, set into the border |
| `▍` | streaming cursor | blinks after the hero name and the prompt line |
| `⏱ ⛓ ▶` | status-bar segments | rendered as `elapsed`, `subagents`, `ctx` |
| `⠋⠙⠹` | busy spinner | drawn as a rotating stroke-dash arc |

Section headings are lowercase (`about`, `stack`, …), the social row is a two-segment chip bar
(`label` on near-black, `message` on a colour), and two sections answer in verbatim
`console` fences. The panel frames its title the way the TUI does — a rule broken by a gap with `▾ profile` inside it —
and that rule sits **inside** the card rather than on its top edge. A rule on the card's own top
edge would need transparent space above it, which means a band of page background across the top
and the top half of the title landing outside the panel.

### Glyph policy — all structural glyphs are drawn, never typed

GitHub renders these SVGs through `<img>`, so nothing can load a webfont — text falls back to the
viewer's system monospace, and the metric of that fallback is not yours to choose. Measured
coverage of the candidates in **Consolas** (Windows, GitHub's code font) and **Consolas Bold**:

- present: `─ │ ┌ ┐ └ ┘ ├ ┤ ┬ ┴ ┼ ● ○ ■ ▸ ▾ ▀ ▄ ░ ▒ ▓ █ · • → ← ↑ ↓ ≡ × √`
- **missing entirely**: `❯ ✓ ✗ ↳ ⏱ ⛓` and the Braille spinner range (`⠋⠙⠹…`)
- **missing in the bold face only**: `╭ ╮ ╯ ╰` (rounded corners) and `┊` (dashed rail)

A missing glyph never fails loudly — the browser substitutes a **proportional** face for that one
character, which tears the monospace grid apart (a `✓` that is wider than a cell makes every
following column drift). So **every** prompt chevron, tool bullet, result check, streaming cursor,
spinner, rail and panel corner in `assets/*.svg` is drawn geometry, not a character.

The same audit drives the README: code fences answer with words (`0 errors · sources live`) and use
`>` for a prompt and `──>` for arrows, staying inside the safe list above. Body text (rendered in
the viewer's UI font, not monospace) may use `·` and `—` freely.

`textLength="340"` pins the hero name in `banner.svg` so the blinking cursor keeps its column
whatever the fallback — Consolas advances 0.55em per character, Menlo and SF Mono advance 0.60em,
which is a 37px swing on a 15-character line.

### Animation rules

- **Every asset's base state is fully legible.** The staggered reveals only *enhance*; if SMIL
  never runs you still see the finished design, not a blank.
- **Transform and opacity only.** No `width`/`height`/`x`/`y` animation (the accent rules draw in
  with `type="scale"` + `additive="sum"`), and no `feGaussianBlur` on large areas.
- **Reveals live on wrapper `<g>` elements, never inside `<text>`** — `animateTransform` inside
  `<text>` interacts badly with `text-anchor`. Two animations on one attribute also conflict, so
  the blinking cursor is a `<rect>` inside a group that owns the reveal.
- **At most two ambient loops per asset** (orb drift + spinner/cursor blink), all
  `repeatCount="indefinite"`, all transform/opacity.

### Surface rules — the assets are screens, not cards

`banner`, `panel` and `mark` each build the same surface in four layers, in this order:

1. **base** — `hbg` / `pscreen` / `mscreen`, a near-black blue gradient (`#0B0E14` → `#040507`).
   Deliberately deeper and cooler than a typical card grey, so it reads as a dark terminal rather
   than a web panel.
2. **phosphor wash** (`hphos` / `pphos` / `mphos`) — cyan at 5.5% falling to 0 by mid-height.
   This is the backlight a real screen throws onto its own top edge; it is what stops the surface
   reading as flat paint.
3. **vignette** (`hvig` / `pvig` / `mvig`) — transparent centre to `#000` at 34%, so the corners
   fall off the way a lit panel does. Without it a large dark rectangle reads as a box, not a screen.
4. **CRT texture** — a 4px-pitch `<pattern>` of 1.2px lines at 1.5% white (`hscan` / `pscan` /
   `mscan`), plus grain (`hgrain` / `pgrain` / `mgrain`) at `opacity="0.16"`.

Both texture layers are drawn **last**, clipped to the card via `hframe` / `pcard` / `mcard` so
the rounded corners survive, and **above** the content — scanlines that stop at the text look like
a background pattern, not like glass.

Two details worth keeping if you retune this:

- **Scanlines are a `<pattern>`, not a filter.** A pattern paints once as plain geometry. A blur or
  turbulence used as a line texture would force a repaint of the whole card on every frame the
  ambient orb drift moves, which is exactly the cost the animation rules above avoid.
- **Grain is `feTurbulence` remapped to white speckle**, `a = 0.8*R - 0.3`, so roughly half the
  field clamps to zero. A plain low-opacity noise rectangle *lifts the whole surface* into grey and
  kills the black; this keeps the darks at true black while still breaking up the gradient banding.
  `feTurbulence` is static, so it rasterises once on load and never repaints.

The orb gradients were reduced to 0.15 / 0.13 (banner) and 0.16 / 0.14 (panel) at the same time.
At their previous strength the vignette fought them and the surface read as a glow; kept subtle,
they work as light spill on a screen.

### Theme safety

The four assets are images, so they carry their own dark surface and look identical in light and
dark mode. Only `divider.svg` is designed to sit on the surrounding page, so its hairline stays
mid-neutral grey (`#7A7F87`) with `gradientUnits="userSpaceOnUse"` — required, because a
horizontal `<line>` has a zero-height bounding box and object-bounding-box gradients never paint
there. The prompt chevron in it is a 50%-opacity cyan accent, decorative in both themes.

### Third-party services in use

All verified live: readme-typing-svg (the streaming tagline), skillicons (the framework rows),
shields.io (status / social / `open to` chips — note `labelColor=0B0B0C` is what makes them read
as two-segment terminal chips). Deliberately avoided — `github-profile-trophy`,
`github-readme-activity-graph`, `github-readme-quotes` and `github-skyline` (currently returning
402/404).

The three social chips were already icon-less and stay that way: `logo=linkedin` is a **dead**
simple-icons slug, so an icon there silently disappears and the row looks uneven.
`logo=githubactions` and `logo=vercel` in the Tooling row *do* render.

## Why there are no stats / trophy / visitor / snake widgets

Research on 2026 profiles is consistent: those widgets are decoration unless the numbers are
large, and small numbers make a profile look weaker, not stronger. The snake workflow that used
to exist was removed for the same reason — this profile leads with **work**, not metrics.

## Optional follow-up: repo polish (high value)

`BDocuLink` and `SmartVote` are linked from the README but have no description or topics, so they
open onto near-blank repo pages. Add a description + topics to each:

- **BDocuLink** — "Barangay document management system: document processing, resident records and
  approval workflows." topics: `laravel`, `react`, `typescript`, `mysql`, `barangay`
- **SmartVote** — "Voting platform: ballot construction, candidacy and result tabulation."
  topics: `laravel`, `blade`, `mysql`, `voting-system`

## Checking a change before you push

```powershell
# every asset must still parse as XML
Get-ChildItem assets\*.svg | ForEach-Object {
  $x = New-Object System.Xml.XmlDocument
  try { $x.Load($_.FullName); "OK   $($_.Name)" } catch { "FAIL $($_.Name): $($_.Exception.Message)" }
}

# every asset the README links to must exist
Select-String -Path README.md -Pattern 'assets/([a-z]+\.svg)' -AllMatches |
  ForEach-Object { $_.Matches } | ForEach-Object { $_.Groups[1].Value } |
  Sort-Object -Unique | ForEach-Object { "$_  $(Test-Path (Join-Path assets $_))" }
```

Then open `README.md` in VS Code's Markdown preview (Ctrl+Shift+V) — it uses the same relative
paths GitHub does, so a broken image shows up locally as a broken image.

