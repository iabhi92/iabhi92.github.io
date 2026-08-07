# iabhi92.online — portfolio site

Static single-page portfolio for Abhinav Kumar (Sydney-based security/backend engineer).
Plain HTML/CSS/JS in `index.html` — no build step, no framework, no bundler.

## Live site

- Primary domain: **iabhi92.online** (set via `CNAME`)
- `iabhi92.github.io` 301-redirects to the custom domain — always check the custom domain when verifying, not the `.github.io` URL.
- Hosted on GitHub Pages, deployed automatically by GitHub Actions on every push to `main`. No manual deploy step.
- If a deploy looks stuck, check `https://www.githubstatus.com/api/v2/components.json` for the `Actions`/`Pages` component status before assuming the repo is broken — there was a genuine multi-hour GitHub-side outage during this project (now resolved).

## Local dev

```
npx serve -l 5050 .
```
from the repo root, then open `http://localhost:5050/`. This repo has no `node_modules` of its own; for browser automation/screenshots during verification, Playwright is available via the sibling project at `/Users/abhinavkumar/shors-rsa-cracker/frontend/node_modules/playwright` — run node scripts from that directory.

## Design system — "Technical Noir"

All tokens are CSS custom properties near the top of `index.html`'s `<style>` block:

- **Background**: pure black `--bg: #000000`. Panels/cards: `--panel: #0a0c10`, `--panel-2: #16191f`. Borders: `--border: #1f2229` (hairline 1px, always — never a shadow).
- **Text**: light-green tinted, not pure white — `--ink: #dae6d2`, `--ink-dim: #b9ccb2`, `--ink-faint: #84967e`. This was a deliberate correction; don't revert to `#ffffff`.
- **Accent**: single functional color, emerald `--accent: #00ff41`, used sparingly (buttons, live-status dots, hover states, headline emphasis). No second accent hue.
- **Type**: `Source Serif 4` for headlines (`--serif`), `JetBrains Mono` for everything else (`--mono`/`--sans`). No other families.
- **Shape**: `--radius: 0px` everywhere — zero border-radius, no soft shadows, no gradients as decoration. Depth comes from 1px borders and tonal layering only.
- **No decorative background texture** — the ambient grid lines and blurred glow orbs that existed earlier were explicitly removed per feedback ("plain dark black background and green color"). Don't re-add them without being asked.

## Standing rule: nothing fabricated

This has been stated emphatically and repeatedly — treat it as non-negotiable, not a style preference:

- All stats on the site (About section: "3 shipped products", "10+ vulnerability classes", "116+ tests", "100% client-side encrypted") are real and must stay real. Never add fabricated numbers, fake clearance levels, fake "CLASSIFIED"/"RESTRICTED"/"LVL_X_ACCESS" badges, or invented research indices — a Stitch/AI design reference used earlier in this project had exactly this kind of fake dossier flavor text, and it was explicitly stripped out.
- GitHub activity/contribution data is pulled from real APIs (`api.github.com/users/iabhi92`, `github-contributions-api.jogruber.de/v4/iabhi92`) — never replace with `Math.random()` placeholder data.
- The status widget calls a real `/api/status` endpoint on the crackrsa.com backend; if it fails (e.g. CORS on localhost, or the free-tier Render backend sleeping), let it show its real "couldn't reach" failure state rather than faking a success state.
- Project card images: the crackrsa.com screenshot is a genuine live capture. The Threshold Signatures and Haven images are stylized AI-generated title-card banners (their embedded dashboard mockups have garbled/nonsensical text) — used deliberately as decorative art, not claimed as literal product screenshots. Alt text says "title card" for those two, not "screenshot", to keep that honest.

## Projects section layout (as of the latest commit)

Each `.project-row` card is a **full-width image banner on top, content below** — not a side-by-side split. This was a deliberate fix: a side-by-side layout forces a landscape image (1376:768) into a tall narrow column, so it either crops badly (`object-fit: cover`) or leaves dead space (`object-fit: contain` in a mismatched box). Stacking the image as a banner means its container is sized by the image's own aspect-ratio (`aspect-ratio: 1376 / 768`), so it always fills edge-to-edge with zero cropping and zero gaps regardless of how tall the text content below it is. If asked to touch this layout again, keep that constraint in mind rather than reintroducing a fixed-height side column.

Images live in `assets/projects/*.webp` (converted from source PNG/JPEG for size).

## Accessibility

Touch targets for icon-only buttons (header GitHub/LinkedIn icons, mobile nav toggle) are 44×44px minimum — this was a real gap found and fixed via the `ui-ux-pro-max` skill's UX guideline lookups, not an arbitrary choice. Keep new icon buttons at that size. Contact form fields have `autocomplete` attributes and visible `*` required-field indicators.

## Useful tool: ui-ux-pro-max skill

Installed globally at `~/.claude/skills/ui-ux-pro-max/` (not project-local). Query it directly for UX guideline checks:

```
python3 "$HOME/.claude/skills/ui-ux-pro-max/scripts/search.py" "<query>" --domain ux -n 8
```

Domains include `ux`, `style`, `color`, `typography`, `product`, `landing`. Useful for a second-opinion pass on contrast, touch targets, form UX, etc. — several real fixes in this project came from it.

## Recent history (most recent first)

- Pushed `--bg` to pure `#000000` for more contrast against panels/text.
- Restacked project cards to image-banner-on-top layout (see above) to fix image fitting.
- Swapped project card images to the user's own three images (real crackrsa capture + two stylized title cards), converted to WebP.
- Fixed touch target sizes and contact form UX via the ui-ux-pro-max skill.
- Redesigned Projects section as "dossier" cards: tags, title, status badge, always-visible Problem/Approach/Result breakdown (previously a click-to-expand toggle).
- Removed decorative ambient grid/orb background texture; retinted body text light-green instead of white; fixed a leftover blue gradient on the crack-widget section that had been missed in an earlier palette pass.
- Full "Technical Noir" retrofit: monochrome + single emerald accent, zero border-radius, Source Serif 4 + JetBrains Mono.

## Other repos referenced from here

- `/Users/abhinavkumar/shors-rsa-cracker` — crackrsa.com, a separate full-stack project (React/FastAPI/Qiskit) linked from this portfolio. Its `frontend/node_modules` is used as a Playwright source for testing this repo (see Local dev above).
