# Review Queue

Items parked for a later look. Newest first. Nothing here is committed to as work.

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
