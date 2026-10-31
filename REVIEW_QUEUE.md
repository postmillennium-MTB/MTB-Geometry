# Review Queue

Items parked for a later look. Newest first. Nothing here is committed to as work.

---

## 2026-10-30 — Mad Scientist MTB: RAD calculator and geometry compare

**Links**
- https://madscientistmtb.com/rad-calc/
- https://madscientistmtb.com/bike-geometry-compare/

**Why queued**
Saved as reference, no stated reason beyond the topic. Not yet opened. Going by the URLs only, they appear to overlap our "Cornering & RAD" and "Compare" tabs.

**To evaluate**
1. RAD: how they define and calculate it, and whether the formula, inputs and target values match ours. Any disagreement is worth resolving before we publish numbers.
2. Compare: how many bikes it handles at once, how it presents the differences (table, overlay, deltas), and where it beats our Compare tab.
3. Data: where the geometry numbers come from, and whether there is a bike database we lack.
4. Gaps: what each tool shows that ours doesn't, and the reverse.

**Guardrail**
Same as the BikeYoke item. Review for ideas and feature gaps, build independently, and don't reuse their code, data or assets.

**Status:** reviewed 2026-10-31

**Findings**

*What they are.* Two spreadsheet-style calculators. Manual entry only, no bike database, desktop-only. The full one also downloads as Excel, and states that manufacturer geometry tables can be wrong.
- RAD calculator: four bikes side by side, no graphics. 13 inputs per bike: reach, stack, head angle, stem length, stem rise angle, stack height to stem clamp, bar rise, backsweep, upsweep, swept-section length, bar roll, grip length, grip diameter. Outputs RAD, RAAD (RAD angle) and SHO (steering/hands offset). The formula is not published on the page.
- Geometry calculator: three bikes (a base bike, an optional modified version of it, and up to two comparisons), with side and top views and a choice of alignment anchor (rear tire on ground, rear axle, BB, mid-saddle). Modifications: fork travel/offset, mixed wheel sizes, angleset, reach-adjust headset, lower headset cup, rear center. Outputs include wheelbase, front center, effective top tube, trail, effective seat angle, seated fit, RAD, RAAD, SHO. (Views and controls read from the page's labels; the plots were not inspected.)

*Our RAD does not match theirs.* The same inputs, run through our formula (`index.html`, `geo()`) and through the Mad Scientist RAD calculator's four default bikes:

| Bike | Their RAD / RAAD | Our RAD / RAAD | Gap |
|---|---|---|---|
| 1 | 874.2 mm / 57.5° | 800.4 mm / 54.2° | -74 mm (-8.4%) |
| 2 | 874.5 mm / 58.5° | 796.3 mm / 54.7° | -78 mm (-9.0%) |
| 3 | 870.2 mm / 58.2° | 791.9 mm / 54.4° | -78 mm (-9.0%) |
| 4 | 878.7 mm / 58.5° | 776.8 mm / 54.0° | -102 mm (-11.6%) |

BikeYoke's default setup (872 mm / 58.5°) shows the same direction: roughly 824 mm / 56.3° from our formula on equivalent inputs. Input mapping is approximate there.
- The gap is 4–5× our ±20 mm "within target" band, so the green/amber/red RAD status is not comparable with either tool.
- Likely cause: we add stem length and bar rise as flat horizontal and vertical offsets. They project stack-height-to-clamp, headset/spacers, stem and bar rise along the tilted steerer axis (head angle), include stem rise angle and upsweep, and locate the grip center. We also use a fixed 370 mm bar length for the sweep correction.
- Check: a quick, uncalibrated reconstruction using those terms lands within 13–27 mm of their RAD on all four bikes (RAAD within about 1°). This supports the cause but is not a finished model.
- Open question: our target is rider height × 4.47 (M) or 4.60 (F). Whether that matches Lee McCormack's published targets is unverified. It may have been tuned to our understated RAD, so check it against the source before changing the formula.

*What they lack.* Neither tool has stability (weave/capsize), cornering lean, weight distribution, wheel flop or the handling radar. Our tool is not a copy-gap there.

*Ideas worth taking.*
1. SHO (we already show RAD and RAAD).
2. Alignment-anchor choice on any overlay.
3. "Modify a base bike" as deltas (fork travel, angleset, reach-adjust headset, rear center).

**Next step if pursued:** fix RAD first (see BikeYoke item for the extra inputs it needs).

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

**Status:** reviewed 2026-10-31

**Findings**
- A version of this already exists. The left sidebar has a "Quick preview" block (`index.html`, around line 2389) with four buttons, one per view (trail vs head angle, stability, cornering, handling radar). Clicking one jumps to that tab.
- It doesn't work as a summary. Each chart is a 64 px-tall chart clipped into a 54×42 px window, so only a faint sliver shows. The radar thumbnail is the only readable one.
- On a phone the block sits above the stat cards and pushes the real chart down roughly 250 px.
- Feasibility is high. The data for all four views (`tmap`, `stabMerged`, `cmap`, `rdComb`) is already computed once at the top level. The chart components (`LC`, `Radar`, `Diag`) take a height, so tiles are mostly layout work.
- Suggested design: summary as the first tab, 2×2 grid on desktop and stacked on phone, each tile clickable to its full tab, charts at about 150–200 px tall. Tiles: wheel diagram plus trail chart; stability chart; lean chart plus RAD readout and weight bar; handling radar plus the four or five biggest A/B deltas. Remove the sidebar Quick preview once it ships.
- Still open: where the editable source lives. Only the compiled `index.html` is in this repo.

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

**Status:** reviewed 2026-10-31

**Findings**

*What it is.* A standalone viewer embedded in their homepage (a WebGL canvas inside an iframe), opened from an "Open cockpit calculator" button. It is a cockpit-position tool only. It models no steering geometry, trail, stability, weight distribution or rider.
- Inputs: frame reach, stack, head tube angle; headset height, total spacers; stem length and rise; bar width, backsweep, upsweep, bar reach, bar stack (as specified); trim per bar end, bar rotation. Each is a slider plus a number box. Quick-pick buttons for their own stem lengths (35/45/55) and bar stack (50/65) make it a product funnel as well as a calculator.
- Outputs: overall reach, overall stack, stack/reach ratio, RAD, RAD angle, effective stem length, plus bar width and sweeps after trim and rotation. Default example: 455 mm / 743 mm / 872 mm / 58.5°.
- The numbers are plain 2D trigonometry: √(455² + 743²) = 871 ≈ their 872 mm, and atan(743/455) = 58.5°. The 3D view adds no extra physics.

*Why it feels better: UX, not math.*
1. **Current vs New** setups drawn as a ghost overlay, with a results table of Current / New / Δ rows.
2. Hover a table row and that measurement is highlighted on the model; "Dimensioned" view; Free / Top / Rear / Front camera presets.
3. Pin toggles, shareable "Copy link", Reset all, and headline tiles (reach, stack, RAD) floating over the model.

*Where we are.* Our Compare tab already has Config A/B with a Δ column, but no overlay. We lack spacers, headset height, stem rise, bar width, upsweep, trim and rotation, which are also the inputs our RAD needs (see the Mad Scientist item).

*Recommendation.* Skip full 3D for now: it needs an inlined three.js plus a bike model, and it adds no numbers. A 2D side-view SVG overlay of two setups, a Δ table with hover highlighting, and a top view for bar width and sweep covers most of the value. Our existing front-wheel diagram shows we already draw SVG.

*Not checked.* BikeYoke's calculator on a phone, and whether it works inside an iframe embed.
