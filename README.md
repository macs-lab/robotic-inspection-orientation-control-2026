# Robotic Inspection Orientation Control 2026

Project website for the ISFA 2026 paper:

**"Admittance-Based Surface Alignment for Human-in-the-Loop Robotic Visual Inspection"**

A real-time, closed-loop robotic orientation control pipeline for precision visual inspection.
The end-effector is modeled as a virtual sphere moving through a viscous medium, so the resulting
mass–damper admittance system unifies perception-driven surface-normal tracking and operator input
into a single compliant motion stream. Validated on a UR5e with eye-in-hand depth sensing
(final mean orientation error 0.4°).

## Authors
Antara Banerjee\*, Colin Acton\*, Xu Chen — University of Washington
(\* equal contribution)

## Links
- **Live site:** https://macs-lab.github.io/robotic-inspection-orientation-control-2026/
- **Video:** https://youtu.be/zu-xbil4paw
- **Paper:** drop the compiled PDF at `static/pdfs/paper.pdf` (the "Paper" button links here)

## Repo layout
```
.
├── index.html              # the project page
├── static/
│   ├── css/                # Bulma + template styles
│   ├── js/                 # bulma-carousel, bulma-slider, index.js
│   ├── images/             # figures and result plots (re-exported from LaTeX/)
│   └── pdfs/               # paper.pdf goes here
├── LaTeX/                  # Overleaf source of the paper (main_jrnl_v2.tex)
└── website_build_guide.md  # the build plan this site follows
```

## Updating the site
- Content lives directly in `index.html` (Bulma CSS, no build step). Edit and commit.
- Result plots are regenerated from the LaTeX PDFs:
  `pdftoppm -png -r 200 LaTeX/<plot>.pdf static/images/<plot>` then rename `-1.png`.
- To publish, push to `main`; GitHub Pages serves the root of the branch.

## Template attribution
Built on the [Nerfies](https://nerfies.github.io) project page template
([source](https://github.com/nerfies/nerfies.github.io)), used under its license. Thanks to the
original authors.

## Website License
<a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/"><img alt="Creative Commons License" style="border-width:0" src="https://i.creativecommons.org/l/by-sa/4.0/88x31.png" /></a><br />This work is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/">Creative Commons Attribution-ShareAlike 4.0 International License</a>.
