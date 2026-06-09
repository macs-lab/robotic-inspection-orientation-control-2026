# Project Website Build Guide (ICRA-Style)

A step-by-step plan students can execute end-to-end. Targets the look of recent ICRA/CoRL/RSS robotics papers (Nerfies-style template), free to host on GitHub Pages, matched to the content of the ISFA 2026 admittance-control paper.

## Repository

A GitHub repo has been created for the website build:

**https://github.com/macs-lab/robotic-inspection-orientation-control-2026**

Students should clone this repo first and treat it as the canonical workspace. All assets, HTML, and CI live here. To enable GitHub Pages: **Settings → Pages → Source: `main` branch, `/` (root)** — the live site will be at:

`https://macs-lab.github.io/robotic-inspection-orientation-control-2026/`

First commits should be:

1. Drop the Nerfies template files into the repo root (preserve their MIT attribution in the footer).
2. Add a `README.md` describing the project and a link back to the paper.
3. Enable Pages and confirm the placeholder deploys before writing any content.

---

## 1. Reference what "good" looks like first

Spend 30 minutes browsing 5–10 recent robotics project pages. Examples to study:

- **Nerfies** — https://nerfies.github.io (the canonical template used by hundreds of robotics papers)
- **PaLM-E**, **RT-2**, **Mobile ALOHA**, **Diffusion Policy**, **Dr. Eureka** project pages

Note the common structure: hero → buttons → video → abstract → method → results → comparisons → BibTeX → footer. Use that structure.

## 2. Tech stack (do not invent your own)

- **Template:** fork https://github.com/nerfies/nerfies.github.io (MIT-licensed, designed for this exact purpose).
- **CSS:** Bulma (already in the template). Don't switch to Tailwind/React — the simple HTML keeps maintenance cheap.
- **Hosting:** GitHub Pages on the repo above.
- **Math:** MathJax (already wired in the template) for any inline equations.
- **Carousel:** Bulma-Carousel (in the template) for the multi-clip results section.

## 3. Repo layout

```
robotic-inspection-orientation-control-2026/
├── index.html
├── static/
│   ├── css/         (bulma.min.css, index.css, fontawesome)
│   ├── js/          (bulma-carousel, index.js)
│   ├── images/      (figures from the paper, exported as PNG @2x)
│   ├── videos/      (short MP4 clips, H.264, ≤10 MB each, 720p–1080p)
│   └── pdfs/        (paper PDF, supplementary)
└── README.md
```

## 4. Asset prep (the part students always underestimate)

Before touching HTML, prepare:

### Videos (most impactful asset)

1. A 30–60 s **headline video** (`hero.mp4`) showing: starting misaligned, controller engaging, end-effector slewing to normal, operator nudging during alignment. Add subtitle text overlays.
2. Three short 5–10 s **clips** for the results carousel: 15° case, 40° saturated case, manual teleop comparison.
3. Encode H.264, AAC, ≤10 MB each. Use `ffmpeg -vcodec libx264 -crf 23 -preset slow -movflags +faststart`. The `+faststart` flag is critical for fast web playback.
4. Generate `.webm` variants for fallback.

### Figures (re-export, don't reuse paper PDFs as-is)

- Re-export every figure as PNG @1.5× resolution with transparent background where applicable.
- The flowchart (`flowchartorientation.png`), graphical abstract (`graphic_abstract.png`), and FBDs (`rigidbody.png`, `FBD.png`) should be redrawn cleanly in Figma/Inkscape — paper-quality figures often look mediocre on the web.
- Add one **animated GIF** of the simulated mass–damper response (loop the 15° trajectory).

### Plots

- Re-render the 4 result plots as SVG (matplotlib `savefig('out.svg')`). SVG scales crisply on retina displays where the PDF screenshots will look fuzzy.

## 5. Page sections (mapped to this paper)

Replace the Nerfies template content section-by-section:

| Section | Content for this paper |
|---|---|
| **Hero** | Title, authors, affiliations (UW ME + ECE), conference badge ("ISFA 2026"), button row: `Paper`, `Video`, `Code` (GitHub if released), `BibTeX` |
| **Teaser** | The 30–60 s headline video, autoplay muted loop |
| **Abstract** | Paste from `main_jrnl_v2.tex` verbatim |
| **Method overview** | Graphical abstract figure + one paragraph; then 3 sub-cards: (a) Perception (RANSAC + PCA normal estimation), (b) Virtual mass–damper admittance, (c) PD outer loop with torque saturation. Each sub-card: small diagram + 2–3 sentences |
| **Results carousel** | 15° clip, 40° saturated clip, side-by-side manual vs. controller comparison. Each clip captioned with the key metric |
| **Plots** | Embed the 4 SVG plot pairs (angle/torque, vel/ω) for 15° and 40° |
| **Comparison** | Small table: this work vs. Nakhaeinia 2018 vs. manual baseline. Make it visually scannable, not just text |
| **Live demo (optional)** | An interactive widget (Plotly/D3) showing how varying `ωn`, `ζ`, `m`, `R` changes the simulated response. The kind of touch that lifts a page above its peers |
| **BibTeX** | Code block with the full `@inproceedings{...}` entry, with a copy-to-clipboard button |
| **Acknowledgments + Footer** | Funding, lab, template attribution to Nerfies |

## 6. Visual polish checklist (the difference between OK and ICRA-tier)

- Single accent color used consistently for links and section dividers (pick from UW purple `#4B2E83` or a neutral robotics blue).
- Sans-serif body (Google Sans or Inter), 17–18 px, line-height 1.55.
- Max content width ~960 px. Resist edge-to-edge layouts.
- Every video must have `autoplay muted loop playsinline poster="…jpg"` and a still poster frame.
- Figures use `loading="lazy"` and explicit `width`/`height` to avoid layout shift.
- Add OpenGraph meta tags (`og:title`, `og:image`, `og:description`) — when shared on Twitter/Slack, the page renders a card with the headline image.
- Run Lighthouse; aim for Performance ≥90, Accessibility ≥95.

## 7. Reproducibility links (high-impact reviewers look for this)

Even if the code isn't open-sourced yet, list what *will* be released:

- ROS 2 Servo pipeline node
- MATLAB simulation script
- D405 calibration + normal-estimation utility
- CSV logs from the 15° and 40° trials

A "Code & Data" section with "coming soon" tags is far better than no section at all.

## 8. AI-Assisted Workflow

Treat the build as a series of AI-assisted passes rather than a fixed schedule. The student team drives the work; AI accelerates the tedious parts. The pattern: students decide *what* needs to exist, AI drafts the first version, students verify and polish.

### Pass 1 — Scaffolding

- Clone the repo locally. Open it in Cursor / Claude Code / Copilot.
- Prompt the AI to **inventory every placeholder** in the Nerfies template (`<!-- TODO -->`, `Lorem ipsum`, sample author names, dummy video URLs) and produce a checklist of files and line numbers to edit.
- Ask the AI to **wire the conference badge and title** in `index.html` using the paper title and ISFA2026 identifier.

### Pass 2 — Copy generation from the paper

- Feed `main_jrnl_v2.tex` to the AI with the instruction: *"Generate web copy for each section listed below. Tighten relative to the paper — drop derivations, keep intuitions. Match the tone of recent ICRA project pages."*
- Sections to request in one prompt: Abstract (one-paragraph version, ~120 words), Method overview (3 sub-cards: perception, admittance, PD), Results blurbs (one sentence each for the 15° and 40° clips), Comparison paragraph.
- Have a second student do an **independent read-through** of the AI output against the paper to catch any hallucinated numbers or claims. Treat this as mandatory.

### Pass 3 — BibTeX and metadata

- Prompt: *"Generate a BibTeX entry for this paper, plus OpenGraph and Twitter Card meta tags. Use the paper title, author list, and `https://macs-lab.github.io/robotic-inspection-orientation-control-2026/` as the canonical URL."*
- Have the AI also draft `alt` text for every figure and `aria-label`s for every button — accessibility is one of the easiest places to lose Lighthouse points and the easiest place for AI to help.

### Pass 4 — Asset generation

- For the redrawn flowchart and FBDs: prompt the AI with a description of each diagram and ask it to generate **clean SVG or Mermaid source**. Students refine in Figma after.
- For the result plots: take the existing MATLAB code and prompt the AI to port it to a Python matplotlib script that exports SVG at the right size for the web column (~900 px wide).
- For video: AI cannot shoot the footage, but it can **write the on-screen subtitle text** and suggest cut points given a description of the trial.

### Pass 5 — Interactive parameter widget (optional but high-leverage)

- Prompt: *"Write a self-contained HTML + JS widget using Plotly.js that simulates the second-order closed-loop response $I_A\ddot\theta + (b+k_2)\dot\theta + k_1\theta = 0$ with sliders for $\omega_n$, $\zeta$, $m$, $R$, $d$. Show the analytical step response. Embed-ready, no build step."*
- Students verify the math against the paper before committing.

### Pass 6 — Review and audit

- Run Lighthouse. Paste the report into the AI and ask: *"Identify the three highest-impact fixes for Performance and Accessibility scores given this HTML. Output as a diff."*
- Run `npx pa11y` for accessibility and feed any flagged issues to the AI for fixes.
- Ask the AI to **review the live page like a reviewer** would: *"You are an ICRA reviewer skimming this project page for 90 seconds. What's missing? What's unclear? What's the most overstated claim?"* Then act on the most important responses.

### Pass 7 — Repo hygiene

- Ask the AI to draft `README.md`, `CONTRIBUTING.md` (so other lab members know how to update), a `LICENSE`, and a GitHub Actions workflow that runs `linkinator` on every PR to catch broken links.
- Add `.gitignore` for `node_modules`, system files, raw video sources.

### Rules of engagement for the AI

- **Never let AI invent numbers**, citation counts, or results. Every metric on the site must be traceable to the paper.
- **Show diffs, not rewrites.** Ask the AI to propose edits as `Edit` operations on existing files rather than regenerating whole files — easier to review.
- **Cap response length** when iterating on copy: *"Reply in under 80 words"* avoids bloated paragraphs.
- **One student owns final approval** of every AI-generated commit. AI is a draftsman, not a decision-maker.

## 9. Common mistakes to avoid

- **Reusing paper PDFs as embedded figures** — they render fuzzy on retina; always re-export.
- **30+ MB hero video** — kills load on mobile; cap at 10 MB and use a poster image.
- **Walls of LaTeX** — the website is *not* a paper rerun. Use 1–2 key equations max; visualize the rest.
- **No video on the hero** — robotics work without motion is unconvincing.
- **Forgetting `playsinline`** — iOS Safari will full-screen the autoplay video without it.
- **Custom CSS framework** — keep Bulma, don't burn a week styling from scratch.

## 10. Reference template repos worth studying

- https://github.com/nerfies/nerfies.github.io — base template
- https://github.com/eliahuhorwitz/Academic-project-page-template — modern Tailwind variant
- Robotics group lab pages at CMU / Stanford / MIT often link out to their group's preferred template

---

**Bottom line:** Clone the Nerfies template into the repo, deploy a placeholder *first*, then fill content into a structure that already works — not the other way around. The repo is the workspace; AI is the accelerator; students remain the editors-in-chief.
