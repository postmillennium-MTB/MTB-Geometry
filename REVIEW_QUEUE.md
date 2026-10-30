# Review Queue

Items parked for a later look. Newest first. Nothing here is committed to as work.

---

## 2026-10-30 — Add a summary landing tab

**Idea**
Make the first tab a summary showing a smaller version of each graphic from the four existing tabs, so everything is visible from one view:
- Geometry
- Stability
- Cornering & RAD
- Compare

**Current state (from this repo)**
- The tab bar in `index.html` is `["diagram", "Geometry"]`, `["stability", "Stability"]`, `["cornering", "Cornering & RAD"]`, `["compare", "Compare"]`. Geometry is the default.
- The headline stat cards already sit above the tabs, so the summary tab must add something they don't.

**To evaluate**
1. Which single graphic best represents each tab (e.g. the side-view diagram for Geometry, the headline chart for the other three).
2. Whether each thumbnail should be clickable and jump to its full tab.
3. Whether the thumbnails can reuse the existing chart components at a smaller size, or need simplified versions (axes, labels and legends get unreadable when shrunk).
4. Layout: a 2×2 grid on desktop, stacked on mobile and in a Pinkbike iframe.
5. Cost: rendering four charts on the first tab means more work on load. Check on a phone.
6. Build: `index.html` looks like compiled output (React inlined, `D(...)` calls instead of JSX). Confirm where the editable source lives before changing it.

**Status:** not started

---

## 2026-10-30 — BikeYoke "Cockpit Calculator" vs. our geometry tool

**Links**
- Ours: https://www.postmillenniumrenaissance.com/geometry/
- Reference: https://bikeyoke.com/en/#3d-cockpit

**Why queued**
Found a 3D visualization of how geometry and cockpit parameters change rider position. Judged to be materially better than our current tool.

**Our baseline (from this repo)**
- `index.html` — "MTB Steering Geometry & Stability — Devinci Troy 2025".
- Single file, React bundled inline, no 3D library, no external JS dependencies (fonts only).

**To evaluate**
1. Inputs: which geometry and cockpit parameters they expose that we don't (e.g. stem length/angle, spacers, bar rise/sweep/backsweep, saddle position, rider dimensions).
2. Outputs: what rider-position metrics they report, and whether they are physically meaningful or only visual.
3. Rendering: 3D vs. our current views. What it would take to match inside our single-file build (three.js inlined adds size; a 2D side-view SVG with a rider skeleton may deliver most of the value).
4. Overlap: our steering/stability analysis (trail, wheelbase, etc.) versus their rider-position focus. These look complementary, so a combined tool may be the better path than copying theirs.
5. Mobile and embed behavior (Pinkbike iframe constraints).

**Guardrail**
BikeYoke is a commercial component brand. Review for ideas and feature gaps only. Build independently and don't reuse their code, assets, or branding.

**Status:** not started
