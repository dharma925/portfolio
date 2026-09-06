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

## Current state (as of 2026-09-01, partially updated 2026-09-06 —
see the note on the postnav chain and the two new bullets below;
this section's prose wasn't otherwise re-audited)

    index.html      home — hero, spec panel, stack, capabilities,
                    experience, projects, writing, about, contact
    projects.html   Featured (MACTrace, AXI4-Lite/APB Bridge, P4Guard,
                    NeuronEase, TiO2 MIM, FCNN) + Earlier work (same
                    .entry format as Featured, not a compact list —
                    see "Earlier work format" below)
    blog.html       writing index — 14 posts (see Blog posts below)
    blog/*.html     the posts themselves
    style.css       all styling, shared by every page (repo root, NOT
                    assets/ — that path never existed in this repo)
    sources/        résumé PDF + images
    sources/neuronease/  real result images extracted from
                    NeuronEase2_END.pptx's embedded media — not
                    recreated, actual ADC-readout output

`about.html`, `xplorations.html`, and the two real `xplorations/*.html`
pages are meta-refresh redirect stubs (into `index.html#about`,
`blog.html`, and `blog/neuronease.html` respectively) — not deleted,
because old `works/` pages still link to `about.html` from their inline
nav script. `works/project_template.html` and
`xplorations/blog_template.html` were unused scaffolding and were
deleted outright (nothing ever linked to them).

**Earlier work format**: `projects.html`'s Earlier Work section uses
full `.entry` articles (title, description, `.readmore` link, `.tags`)
— same as Featured, not the old compact `.archive` list. For a project
with no real content yet (see `works/` below), use an honest
"Write-up pending" `.entry-when` and a one-line placeholder description
instead of a `.readmore` link — don't link out to a page that still has
Lorem ipsum on it.

### `works/` pages — real vs. still-placeholder

Of the five original `works/*.html` pages:

- `works/FPGA_Image_processor.html` — rebuilt with real content from
  its GitHub README (github.com/dharma925/FPGA-image-processor).
  Honestly marked "in progress" — the repo says integration is
  incomplete.
- `works/FPGA_NN.html` — retired to a redirect into
  `blog/fcnn-fpga.html`. The GitHub repo behind it
  (github.com/dharma925/NN-on-FPGA, project name `digit_classifier`) is
  the same underlying project as that post, already covered there in
  full — don't maintain a duplicate.
- `works/FPGA_Snake_Game.html`, `works/risc_v_processor.html`,
  `works/x86_processor.html` — **still have Lorem ipsum.** Their linked
  GitHub repos (`FPGA-Snake-game`, `X86-Processor`, `RISC-V-processor`)
  all 404 — no public source to write real content from. Dharma said
  (2026-08-31) he'd send real details/repo links later. Don't invent
  content for these — ask, or check if he's sent details since.

---

## Design system

Everything is CSS custom properties in `:root` at the top of
`style.css` (repo root). Change values there, not in individual rules.

- **Type**: IBM Plex Sans (body), IBM Plex Sans Condensed (headings),
  IBM Plex Mono (labels, metadata, code). Loaded from Google Fonts.
- **Palette**: cool greys on near-white; teal `--pass` (#0E7C86) as the
  single accent.
- **Signature element**: the "Stack" band right under the hero
  (`.flow` wrapper + `.caps`/`.cap`/`.tags` inside it) — languages/
  tools grouped by category, pulled from the résumé's skills list. This
  replaced an earlier "Lint → CDC → Simulation → Report" flow-strip
  concept (with `.stages`/IntersectionObserver scroll-reveal) that
  Dharma said didn't read well; that CSS/JS was removed, not just
  unused. Don't resurrect `.stages` — it no longer exists.
- **Layout**: hairline grids via a 1px border on each cell (`.caps`,
  `.archive`, `.contact-list`, `.shot-grid` all do this). Don't use the
  `--rule`-background-peeking-through-the-gap trick — with
  `grid-template-columns:repeat(auto-fit,…)` and an item count that
  doesn't evenly fill the last row, auto-fit still reserves the empty
  column tracks and the container background shows through as visible
  blank grey cells. Per-cell borders don't have that failure mode.
- **Diagrams**: Mermaid, loaded via CDN as an ES module and initialized
  inline at the bottom of whichever post needs it (see `blog/p4guard.html`,
  `blog/neuronease.html`, `blog/fcnn-fpga.html` for the exact init
  block/theme vars) — not a site-wide include. Copy that pattern for any
  new post that needs a diagram.
- No dark mode currently. If added, do it with a
  `prefers-color-scheme` block overriding the `:root` variables.

### Available classes for project pages

    .shot / .shot img / .shot figcaption   single figure with caption
    .shot-grid                             multi-image grid
    .entry--media                          entry with an image beside it
                                           (stacks below 52rem)
    .prose                                 article body (blog posts)
    .tags                                  monospace tech chips
    .postnav                               prev/next post nav (flexes
                                           two links to opposite ends;
                                           fine with just one)

---

## Content rules — important

**1. No TI internal detail on the public site — even under a new name.**

Internal tool codenames (Atom8, RegaBot, Tron), real TI infrastructure
specifics (e.g. internal VM pool / DNS details), and internal efficiency
metrics tied to TI's actual team ("2 person-months saved", "6x MTTR")
stay on the *résumé* — a private document — and off the public site.
He still works at TI; a public, indexed page describing internal
systems is a different exposure than a résumé or an interview.
Renaming a tool doesn't fix this — employer + dates are public, so
anyone at TI would still recognise the system.

**The resolved pattern (confirmed with Dharma 2026-08-31):** when the
résumé lists TI-built systems with real internal detail — as it now
does, six of them, under "Professional Experience" — write the
corresponding blog posts as Dharma's own **standalone personal
rebuilds** of the same architecture/pattern, not descriptions of TI's
actual systems. Concretely:
- `blog/agentic-orchestrator.html`, `blog/regression-lifecycle-manager.html`,
  `blog/semantic-debug-agent.html`, `blog/workspace-health-checker.html`,
  `blog/release-quality-gate.html`, `blog/chatops-orchestrator.html`
  all share one fictional world with `blog/p4guard.html` (the same
  stand-in chip, LSF-shaped farm, Slack notifier, mock EDA tools) and
  are explicitly kicker-tagged "Personal project."
- No real TI infra names (no Infoblox/VM-pool-style specifics), no
  internal metrics attributed as TI's measured results. Grep for
  "Texas Instruments", "Infoblox", "Webex", "Outlook", "Confluence"
  (as a literal product name), "Atom8", "RegaBot", "Tron", "Ralph"
  before considering any of these six posts done.
- If adding a 7th post in this family, follow the same pattern.

**2. Nothing goes on the site — or in the résumé — that isn't actually
built and verified.**

P4Guard (verified against a real local Helix Core server), the six
"Personal project" posts above, NeuronEase, and FCNN on FPGA are all
written from and cross-checked against real source (a real project
deck, or the actual pushed RTL/repo) — not from a plan or a résumé
bullet taken at face value. Two known open issues from this rule:

- **P4Guard** is not yet linked to a public GitHub repo from its post
  (source lives outside this repo). Don't guess the URL — wait for it.
- **FCNN on FPGA / the résumé's "extended into an on-chip
  image-processing pipeline" claim**: when the actual NN-on-FPGA repo
  RTL was read directly (2026-09-01), `convolution_layer.v` and
  `maxpool_layer.v` turned out to be **empty stub files** — no ports,
  no logic, nothing wired to them. That image-processing extension is
  NOT in this repo. Dharma confirmed (2026-09-01) it exists elsewhere
  and will send a link later. Until that link exists, don't put that
  claim back on `blog/fcnn-fpga.html` — and flag to Dharma that the
  same claim is still sitting on the actual résumé PDF
  (`sources/Dharma_Resume_DVAI.pdf`), unverified.

**3. Don't invent project detail — go find the real source instead.**

When real detail is missing, the fix demonstrated twice now is to go
get it, not invent it or leave it thin:
- NeuronEase was rewritten after reading `NeuronEase2_END.pdf` and
  extracting real result images from `NeuronEase2_END.pptx`'s embedded
  media (both live in `~/Desktop/Resume/`, one level up from this repo).
- FCNN on FPGA was rewritten after reading the actual pushed Verilog in
  github.com/dharma925/NN-on-FPGA directly (not just the README).

For the three still-placeholder `works/` pages (Snake Game, RISC-V,
x86), that source doesn't exist publicly — their linked repos 404. Ask
Dharma rather than filling the gap with plausible-sounding specifics.

---

## Blog posts

`blog.html` lists all of these; the postnav chain (prev/next links at
the bottom of each post) runs:
`p4guard → agentic-orchestrator → regression-lifecycle-manager →
semantic-debug-agent → workspace-health-checker → release-quality-gate →
chatops-orchestrator → neuronease → mim-fabrication → fcnn-fpga →
mac-trace → axi-apb-bridge`
(this doc previously stopped the chain at `fcnn-fpga`, but `mac-trace`
was already wired in after it in the actual files — fixed here
2026-09-06 when `axi-apb-bridge` was appended.)
`pre-submit-quality-gates` and `regression-reports` are a separate
two-post chain (essays, not in the project chain above).

**Two general engineering essays** — demonstrate judgment without
disclosing anything about TI:
- `blog/pre-submit-quality-gates.html`
- `blog/regression-reports.html`

**Project writeups, real content, all live:**
- `blog/p4guard.html` — see Content rule #2 (public repo link pending).
- `blog/agentic-orchestrator.html` through `blog/chatops-orchestrator.html`
  (6 posts) — see Content rule #1. Personal-rebuild framing, no TI trace.
- `blog/neuronease.html` — 2×2 SPICE proof → box-blur kernel → INT8 CNN
  (12 3×3 kernels, 20,410 params) on a 9×8 crossbar, MNIST results, real
  cycle-count stats (9 cyc/kernel, ~12 cyc/pixel). Advisor: Dr. Srinivasu
  Bodapati. Has a Mermaid architecture diagram (abstracted, not the
  literal schematic — Dharma asked not to "put the solution openly") and
  6 real result images in `sources/neuronease/`.
- `blog/mim-fabrication.html` — TiO2 MIM cells. Real numbers: ~97nm
  TiO2, V_SET 4.1V, I_on/I_off 10^4 (Cu) vs 10^2 (Al), Al underperformed
  due to native Al2O3 at the interface.
- `blog/fcnn-fpga.html` — 256→128→10 FCNN, hand-instantiated neurons
  (128 + 10, not a generate loop), hardware ReLU inside each neuron, a
  real iterative Taylor-series hardware exponential behind softmax
  (argmax decision, since softmax is monotonic — the Taylor unit's
  actual probabilities aren't on the decision's critical path). Zybo Z7.
  Verification depth stated honestly: only `adder` has a real testbench.
  See Content rule #2 re: the image-processing extension NOT being in
  this one.
- `blog/mac-trace.html` — C++17 CNN accelerator model, golden functional
  model kept separate from a documented cycle model, bit-exact (max
  logit diff 0) against an independent NumPy reference on 20 MNIST
  images. Real measured numbers: 96.46%/96.43% float32/INT8 accuracy,
  100%/20.8% MAC utilization (conv vs. FC layer), 11,662 estimated
  cycles on a 4×4 array. (Not previously listed in this doc, though the
  file was already live — added here 2026-09-06.)
- `blog/axi-apb-bridge.html` — added 2026-09-06. AXI4-Lite-to-APB
  bridge (single-outstanding FSM, address decode in the bridge, SLVERR
  on out-of-range) plus an original GPT timer peripheral (RW/RO/W1C/
  staged-write register mix). Hand-rolled UVM-styled SystemVerilog env
  (no real `uvm_pkg` — Verilator can't compile Accellera's uvm-core;
  said plainly on the post) — scoreboard cross-checks AXI, APB, and an
  independent reference model. 8/8 tests, 0 scoreboard errors, 94%/89%
  coverage (random/combined-directed), 6 SVA, real waveform image at
  `sources/axi-apb-bridge/waveform.png`. Two real bugs documented on
  the post: a blocking/nonblocking assignment race in the testbench,
  and a genuine Verilator convergence bug confirmed on two Verilator
  versions (5.020 packaged, 5.038 from-source) before working around
  it. Source: `github.com/dharma925/Basic-Processor`, subdirectory
  `axi4lite_apb_bridge/`, branch `claude/axi4-lite-apb-bridge-uvm-q6lmq4`
  — **not yet merged to that repo's main**, so the post and this doc
  link the branch path directly; re-check whether it's been merged
  before assuming the branch link is still needed.

To add a post: copy an existing file in `blog/` as a template (match its
nav block exactly), then add an entry to `blog.html`'s list and wire it
into the postnav chain at the right point.

---

## Known facts (use these, don't re-derive)

    Name        Dharma Teja Gurram
    Email       gurramdharma925@gmail.com
    Phone       +91 9182436977
    LinkedIn    linkedin.com/in/dharma925
    GitHub      github.com/dharma925
    Location    Bengaluru, India

    TI          Design Verification Engineer, Jul 2024 – present
    TI intern   Digital Design Intern, Jan – Jul 2023
                UCD3138 digital power supply controller
    Education   B.Tech EE (Hons), IIT Mandi, 2020–2024, CGPA 8.68

    Résumé PDFs (two, as of 2026-09-03 — see "Résumé section" below):
                sources/Dharma_Resume_DVAI.pdf   — Hardware & Silicon track
                sources/Dharma_Resume_DVAIC.pdf  — AI × Hardware track
                An older sources/Dharma_Resume_CLG_INTERN.pdf also
                exists (renamed from the original tracked file) but
                nothing on the site links to it.

### Résumé section (index.html#resume)

Added 2026-09-03: two résumé tracks, not one. DVAI was already
submitted for a front-end integration/EDA role; DVAIC is for an
"Architect — AI-Powered Performance Verification Automation" role and
leads with the AI/agentic-systems framing (reordered skills/summary,
MACTrace listed first under Projects, "Multi-Agent Workflow Playground"
instead of "State-Aware Agentic Workflow Orchestrator" as the first TI
bullet — otherwise the same six sub-bullets).

Every résumé link site-wide — the nav `.btn`, the hero's "Download
résumé" button, and every blog/project page's nav — now points at
`index.html#resume` (or `../index.html#resume` from `blog/`), matching
the existing pattern for the `About` nav link. That section is a
`.caps` grid of two `.cap.cap--resume` cards (Hardware & Silicon / AI ×
Hardware), each with its own real PDF link and `target="_blank"`. Only
those two links in the whole site open a PDF directly — everything else
routes through the picker. If a 3rd track is ever added, follow this
same pattern rather than reintroducing a single default download link.

GitHub repos that actually exist publicly under dharma925 (checked
2026-08-31): `portfolio`, `FPGA-image-processor`, `NN-on-FPGA`, plus a
couple of unrelated forks/test repos. `FPGA-Snake-game`,
`X86-Processor`, `RISC-V-processor` do not exist publicly (404) —
see the `works/` section above before assuming otherwise; check again
if enough time has passed that Dharma may have pushed them.

---

## Open work

1. **Snake Game / RISC-V / x86** — still Lorem ipsum on their `works/`
   pages, excluded from Earlier Work's real entries pending real
   detail. Ask Dharma / check if repos are public yet.
2. **P4Guard's public repo link** — not added yet, source not pushed.
3. **The image-processing extension for FCNN on FPGA** — Dharma said
   (2026-09-01) it's a separate piece of work, link coming. Add it to
   `blog/fcnn-fpga.html` once linked; flag that the résumé PDF still
   states this claim unverified against the NN-on-FPGA repo.
4. Project images for the TiO2 MIM post — `.shot`/`.shot-grid` ready,
   no images sourced yet (unlike NeuronEase, which now has real ones).
5. **AXI4-Lite to APB Bridge's branch not merged** — `blog/axi-apb-bridge.html`
   links `github.com/dharma925/Basic-Processor` on branch
   `claude/axi4-lite-apb-bridge-uvm-q6lmq4`, not `main`. Check whether
   it's been merged since 2026-09-06 and repoint the link (and the
   entry in `projects.html`) to the plain repo/subdirectory path once
   it has, rather than leaving a working-branch link live indefinitely.
