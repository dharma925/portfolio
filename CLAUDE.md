# CLAUDE.md — working notes for this repo

Context for Claude Code sessions on this portfolio. Read this first.

---

## Who this is for

Dharma Teja Gurram — Design Verification Engineer at Texas Instruments,
Bengaluru. B.Tech EE (Hons), IIT Mandi, 2020–2024, CGPA 8.68.

The site's job right now is to support applications for **front-end
integration / CAD / EDA automation** roles. That means CI pipelines,
build automation, quality gates, regression infrastructure, Perforce.
When there's a choice about what to emphasise, emphasise that.

---

## Current state

Rebuilt and actually committed to the working tree as of 2026-08-29:

    index.html    home — hero, spec panel, flow strip, capabilities,
                  experience, projects, writing, earlier work, about, contact
    blog.html     writing index
    blog/*.html   4 posts (see below) — a 5th, fpga-neural-network.html,
                  is still open work
    style.css     all styling, shared by every page (at repo root, NOT
                  assets/ — that path never existed in this repo)
    sources/      resume PDF + images

Not yet rebuilt — still the original 4-year-old pages:

    works/*.html        5 project pages, linked from "Earlier work"
    xplorations.html    linked from "Earlier work" (not currently in nav)
    xplorations/*.html  2 more old pages, not linked from anywhere current

`about.html` is a redirect stub (meta refresh to `index.html#about`), not
deleted — 11 old pages under `works/` and `xplorations/` still link to it
from their inline nav script, and rewriting all of those wasn't in scope
for this pass. If those pages ever get rebuilt, delete `about.html` for
real and drop the redirect.

Content on the home page and in the two project blog posts was pulled
from a real resume (RESUME2026.pdf) reviewed 2026-08-29 — not invented.
The resume's placeholder identity fields (sample had `atim@gmail.com`,
`NAME`, etc.) were swapped for the real values below.

---

## Design system

Everything is CSS custom properties in `:root` at the top of
`style.css` (repo root). Change values there, not in individual rules.

- **Type**: IBM Plex Sans (body), IBM Plex Sans Condensed (headings),
  IBM Plex Mono (labels, metadata, code). Loaded from Google Fonts.
- **Palette**: cool greys on near-white; teal `--pass` (#0E7C86) as the
  single accent. The teal reads as "pass" in a build-status sense, which
  is deliberate — it matches the domain.
- **Signature element**: the flow strip on the home page (Lint → CDC →
  Simulation → Report). Stages resolve left-to-right on scroll via
  IntersectionObserver, and are all shown immediately under
  `prefers-reduced-motion: reduce`. Keep that behaviour if you touch it.
- **Layout**: hairline grids via a 1px border on each cell (`.caps`,
  `.archive`, `.contact-list`, `.shot-grid` all do this). Earlier drafts
  used a `--rule`-background-peeking-through-the-gap trick instead —
  don't go back to that: with `grid-template-columns:repeat(auto-fit,…)`
  and an item count that doesn't evenly fill the last row, auto-fit still
  reserves the empty column tracks, and the container background shows
  through them as visible blank grey cells. Per-cell borders don't have
  that failure mode. `.stages` is exempt — it's a fixed `repeat(4,1fr)`
  with always exactly 4 items, so it's safe as-is.
- No dark mode currently. If added, do it with a
  `prefers-color-scheme` block overriding the `:root` variables.

### Available classes for project pages

    .shot / .shot img / .shot figcaption   single figure with caption
    .shot-grid                             multi-image grid
    .entry--media                          entry with an image beside it
                                           (stacks below 52rem)
    .prose                                 article body (blog posts)
    .tags                                  monospace tech chips

---

## Content rules — important

**1. No TI internal detail on the public site.**

Internal tool codenames (Atom8, RegaBot, Tron), architecture specifics,
and internal efficiency metrics ("2 person-months saved", "6x MTTR") are
on the *resume*, which is a private document handed to a named recruiter.
They are deliberately **not** on the public site.

He still works at TI. A public, indexed page describing internal systems
is a different exposure from a resume or an interview. The site describes
*capabilities* — CI ownership, quality gates, regression reporting — not
named internal systems.

Renaming the tools does not solve this. The employer and dates are on the
site; anyone at TI would recognise the system regardless of the name.

**2. Nothing goes on the site that isn't built yet.**

There is a Perforce project ("P4Guard" — p4python triggers,
stream/workspace tooling, build-health reporting). As of 2026-08-29 it
was **in progress, ~4 hours from done** per Dharma — not finished at the
time this pass of the site was built, so it was deliberately left off
both the home page and the two new blog posts. It's fine on Dharma's
*actual* resume once it's real (his call, on his own timeline). Once it's
actually built, add it to the site too — write it from what was actually
built, not from the plan, and check whether it's still true that it's
"not built yet" before assuming this note is current.

**3. Don't invent project detail.**

The five `works/` pages currently all share the same description text
(the memristor blurb, copy-pasted). They need real content, but that
content has to come from Dharma — don't generate plausible-sounding
specifics for the FPGA image processor, snake game, RISC-V core, or x86
core. Ask.

---

## Blog posts

Two are general engineering essays — they demonstrate judgment relevant
to the target roles without disclosing anything about TI:

- `blog/pre-submit-quality-gates.html` — why pre-submit beats
  post-submit, what earns a place in a blocking gate, rejection messages
  as UI, fail-open vs fail-closed.
- `blog/regression-reports.html` — new vs known failures, clustering by
  cause, flakiness, coverage as trend, push vs pull.

Two are his own project writeups, with real data (both written and live
as of 2026-08-29):

- `blog/neuronease.html` — memristor crossbar, analog MAC, Stanford
  ReRAM model, Cadence Virtuoso, Verilog-A. No performance numbers
  (accuracy, crossbar size, energy) were available when this was
  written — don't invent them if you're tempted to fill the gap; ask
  Dharma and add them if he has them.
- `blog/mim-fabrication.html` — TiO2 MIM cells. Real numbers:
  ~97nm TiO2, V_SET 4.1V, I_on/I_off 10^4 (Cu) vs 10^2 (Al),
  Al device underperformed due to native Al2O3 at the interface.

Still open (not yet written — see Open work):

- `blog/fpga-neural-network.html` — FCNN on Zybo Z7, 16-bit fixed point,
  ~661 LUTs for a 256-input neuron.

To add a post: copy an existing file in `blog/` as a template, then add
an entry to the list in `blog.html` and (if it should be featured) to the
Writing section of `index.html`.

---

## Known facts (use these, don't re-derive)

    Name        Dharma Teja Gurram
    Email       gurramdharma925@gmail.com
    Phone       +91 9182436977
    LinkedIn    linkedin.com/in/dharma925
    GitHub      github.com/dharma925
    Location    Bengaluru, India

    TI          Design Verification Engineer, Jul 2024 – present
                (official designation is "Design Verification Engineer")
    TI intern   Digital Design Intern, Jan – Jul 2023
                UCD3138 digital power supply controller
    Education   B.Tech EE (Hons), IIT Mandi, 2020–2024, CGPA 8.68

    Resume PDF  sources/Dharma_Resume.pdf  ← ACTUAL current filename
                (nav + contact + hero CTA all link to this exact path;
                this is still the OLD resume file — see Open work #0)

---

## Open work

0. **`sources/Dharma_Resume.pdf` is still the old resume.** The site
   links to it everywhere (nav, hero CTA, contact), but it hasn't been
   replaced with the actual RESUME2026-derived PDF (real contact info,
   not the sample's placeholders). Get the real filled PDF from Dharma
   and drop it in at that same path — don't regenerate/retypeset it from
   scratch unless asked.
1. Rebuild the five `works/` pages in the new design. Needs real project
   detail from Dharma first.
2. Write `blog/fpga-neural-network.html` (see Blog posts above) and add
   it to `blog.html` and the Writing section of `index.html`.
3. Add project images for NeuronEase and the TiO2 MIM post — `.shot`
   and `.shot-grid` are ready, no images exist yet. Compress before
   committing — GitHub Pages serves them raw.
4. Add the Perforce project (P4Guard) once it's actually built — see
   Content rule #2. Check with Dharma whether it's done before assuming
   the "not built yet" state above is still current.
5. Rewrite or retire `xplorations.html` and the `xplorations/` pages.
   4 years old, describes interests, not work; not linked from current
   nav but still reachable directly and still cross-links to `about.html`.
6. Once 1 and 5 are done and nothing links to `about.html` anymore,
   delete it for real instead of leaving the redirect stub.
