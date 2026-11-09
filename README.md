<!--
Create a stand-alone html file. 

Note that this requires pandoc, which is installed separately (i.e., not a Python package): https://pandoc.org/installing.html

pandoc README.md -o README.html --embed-resources --standalone --mathjax="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-svg.js" -c style.css --metadata title="Snap-Fit Box README"

-->


# Parametric enclosure w/ snap-fit lid

<img src="./docs_for_README/Fusion_logo_new.jpg" alt="" style="max-width:100%; width:160px; height:auto;" />

I created an Autodesk Fusion design for a generic enclosure with a snap-fit lid. 

**Note:** This is based on a previous design (https://github.com/bdubs-astro/enclosure_w_snap_fit_lid), modified to reduce the lid retention strength to make it easier to remove.

<img src="./docs_for_README/x-section v3.png" alt="" style="max-width:100%; width:800px; height:auto;" />

## License

---

This project is licensed under **CC BY-NC-SA 4.0**.

https://creativecommons.org/licenses/by-nc-sa/4.0/

## Design overview

---

This parametric enclosure consists of an open-top box and a removable snap-fit lid. A continuous shallow locating rim aligns the lid with the box and supports its perimeter. Four tapered snap tabs, two on each long side, engage matching grooves to retain the lid. The short ends have no tabs, leaving room for connectors and other internal hardware.

The tabs have reinforced roots that taper toward their free tips. A continuous inside rim-to-lid fillet and rounded transitions at the tab-to-rim junctions distribute the attachment across smooth surfaces. Lateral fit, vertical seating, and bead-to-groove clearance are controlled independently so they can be adjusted for the intended material and fabrication process.

There are two beveled notches in the underside of the lid, midway between the tabs on the long sides, intended to provide entry points for a guitar-pick-like opener tool. The lid can be lifted progressively from these points to disengage the snaps.

📌 Open **Modify → Change Parameters** in Fusion to adjust the model. Formula-based dimensions update automatically; replace an expression only when independent control is intended. All dimensions below are in millimeters.

In the example shown above, the overall dimensions are **110 × 60 × 30 mm** (box length × width × height, excluding the lid), with **2 mm walls** and a **2 mm-thick lid**. The locating rim is **2 mm wide and 2 mm deep**. Tab roots are **2 mm thick**, tapering to **1.25 mm tips**, with **1 mm inside rim and tab-root fillets**. Each tab is **13.75 mm wide**, with centers **27.5 and 82.5 mm** along each long side.

## ⚙️ Box dimensions and edge radii

---

### `length`

```text
110 mm
```

Current value: **110 mm**. Outside box length; the lid and snap positions follow.

### `width`

```text
60 mm
```

Current value: **60 mm**. Outside box width; the lid and locating rim follow.

### `height`

```text
30 mm
```

Current value: **30 mm**. Outside box height, excluding the lid plate and any vertical seam gap.

### `wall_thickness`

```text
2 mm
```

Current value: **2 mm**. Side-wall and bottom thickness. Drives the default reinforced rim, tapered tabs and pry-notch dimensions.

### `inside_fillet`

```text
wall_thickness / 2
```

Current value: **1 mm**. Rounds the inside vertical and bottom box edges. Also sets the reference for the locating rim corner radius.

### `outside_fillet`

```text
3 mm
```

Current value: **3 mm**. Rounds the vertical outside corners of both the box and lid plate.

### `bed_chamfer`

```text
0.25 mm
```

Current value: **0.25 mm**. Anti-elephant-foot chamfer parameter used by Chamfer1. Inspect the selected edges in Fusion when changing it; it is separate from the pick-entry bevels.

## Lid fit and locating rim

---

### `lid_gap`

```text
0.2 mm
```

Current value: **0.2 mm**. Lateral clearance per side between the locating rim/tab outer faces and the box cavity. The vertical seam and bead-to-groove clearance are controlled independently by `lid_seam_gap` and `snap_clearance`. Increasing it loosens lateral guidance and reduces bead engagement.

### `lid_seam_gap`

```text
0 mm
```

Current value: **0 mm**. Vertical gap between the lid plate underside and the box rim. Zero seats the lid on the rim in the CAD assembly. Printed fit still depends on tolerances.

### `snap_clearance`

```text
0.05 mm
```

Current value: **0.05 mm**. Radial clearance between each bead and its groove, independent of lid_gap. This clearance limits vertical play. A positive clearance remains, so zero seam gap does not guarantee zero printed movement. Smaller values may bind; larger values allow more play.

### `lid_thickness`

```text
wall_thickness
```

Current value: **2 mm**. Lid plate thickness.

### `rim_depth`

```text
wall_thickness
```

Current value: **2 mm**. Depth of the continuous locating rim below the lid underside. This is the only locating feature at the short ends; there are no deep short-end tabs.

### `rim_thickness`

```text
wall_thickness
```

Current value: **2 mm**. Width of the continuous locating rim, which supports the tab attachments. It also drives `snap_root_thickness`.

### `rim_corner_radius`

```text
inside_fillet - lid_gap
```

Current value: **0.8 mm**. Outer locating-rim corner radius, matching the box cavity with clearance. Must remain positive.

### `rim_inside_fillet`

```text
wall_thickness / 2
```

Current value: **1 mm**. Continuous fillet around the inside rim-to-lid-plate junction. This is separate from the fillets where the tabs emerge from the rim.

## Four long-side snaps

---

### `snap_offset`

```text
length / 4
```

Current value: **27.5 mm**. Distance from each short end to a tab center. On each long side the centers are snap_offset and length - snap_offset. At the current length these are 27.5 and 82.5 mm.

### `snap_length`

```text
length / 8
```

Current value: **13.75 mm**. Width of each individual tab along the long wall. There are two tabs per long side, four total.

### `lip_depth`

```text
4 * wall_thickness
```

Current value: **8 mm**. Total tab projection below the lid underside. The flexible portion below the locating rim is lip_depth - rim_depth, currently 6 mm, before allowing for the rounded transitions and bead.

### `lip_radius`

```text
3 * wall_thickness / 8
```

Current value: **0.75 mm**. Radius of the semicircular retaining bead. Nominal radial engagement is `lip_radius - lid_gap`, currently **0.55 mm**. Matching groove radii are `lip_radius + snap_clearance`, currently **0.8 mm**. Larger engagement generally requires more deflection to engage or release; verify actual retention and release force with a full-size print.

### `snap_root_thickness`

```text
rim_thickness
```

Current value: **2 mm**. Tab thickness where it joins the bottom of the locating rim. Keep linked to rim_thickness so the root meets the rim at full width.

### `snap_thickness`

```text
5 * wall_thickness / 8
```

Current value: **1.25 mm**. Tab thickness at its free tip. Each tab tapers from `snap_root_thickness` at the rim to this value at the tip.

### `snap_root_fillet`

```text
wall_thickness / 2
```

Current value: **1 mm**. Fillets at both ends of each tab where it emerges from the rim: eight transitions in total. These round the rim-to-tab attachment junctions.

## Beveled pick-entry notches

---

### `pry_width`

```text
5 * wall_thickness
```

Current value: **10 mm**. Width of each of the two beveled pick-entry notches. One is centered at length / 2 on each long side, between the two tabs.

### `pry_depth`

```text
lid_thickness / 3
```

Current value: **0.667 mm**. Height of the opening at the lid outer edge. The underside bevel fades to zero inward, leaving the remainder of the lid plate intact.

### `pry_reach`

```text
wall_thickness
```

Current value: **2 mm**. Distance the bevel extends inward from the outside lid edge. Keep it shorter than wall_thickness + lid_gap so it stops before the locating rim.

## 🔧 Opening and closing the lid

---

To close, align the locating rim with the cavity and engage the four snaps progressively. The lid plate is intended to seat on the box rim with no nominal vertical seam gap. 

⚠️Do not attempt to force a snap requiring excessive pressure.

To open:

1. Locate one of the beveled notches at the midpoints of either of the long sides.
2. Insert a thin guitar-pick-like opener tool into the beveled seam entry.
3. Wedge or lift gently to separate that side progressively and disengage the snaps.
4. Move along the seam and work around the remaining snaps as needed. Avoid forcing a closed corner or aggressively twisting against a sharp edge.

The bevel identifies the insertion point and provides a sloping entry instead of requiring the tool to be forced into an unrelieved edge. It does not guarantee that excessive prying force cannot damage the printed part.

## Full-size testing and model bodies
---

The model contains two bodies:

- `Parametric open-top box`
- `Snap-fit lid`


Print the box and lid from the same set of parameter values so their beads and grooves match. Test the full-size assembly for rim fit, retention, rattling, release force and repeated opening. No measured release-force value is specified.

<div style="break-after: page;"></div>

## 🖨️ Printing

---

Material: **PETG**.

- **Box:** bottom flat on the bed, opening upward.
- **Lid:** outside plate face flat on the bed, tabs upward.
- Use a validated material profile, layer height, wall count, temperature and cooling settings consistently between fit tests and finished parts. This document does not prescribe numerical slicer settings.
- Export the two full-size bodies separately.
- `bed_chamfer` controls the model's anti-elephant-foot chamfer; inspect the actual selected edges in Fusion rather than assuming every bed-contact edge is chamfered.

CAD regeneration does not establish printed layer strength. The wide rim, full-thickness roots, taper and fillets support the tab-to-rim junctions, but physical testing is required. Inspect roots for cracking, whitening or permanent bending during testing.

## 💡Adjustment guide

---

| Symptom | Controls to consider |
|---|---|
| Rim binds laterally | Increase `lid_gap` slightly; this also reduces bead engagement. The seam gap and groove clearance are controlled separately. |
| Visible vertical seam | Check `lid_seam_gap`; the current value is zero. Also inspect printed warping, bed-edge bulging and incomplete seating. |
| Lid rattles vertically | Check `snap_clearance` and that the plate seats on the rim. A zero seam gap alone does not eliminate groove play or manufacturing tolerances. |
| Retention/release force too light | Increase `lip_radius` cautiously, or reduce `lid_gap` if lateral fit permits. Retest the full assembly. |
| Release force excessive | Reduce `lip_radius`, consider more flexible tab length through `lip_depth`, or adjust fit clearances cautiously. Recheck retention and hardware clearance. |
| Pick cannot enter | Adjust `pry_depth` or `pry_width`. Check `pry_reach` while preserving the locating rim and remaining lid thickness. |
| Root cracks | Inspect layer bonding and the failure location; review root/rim dimensions and fillets. Do not assume more bead engagement can be added without increasing load. |
| Tabs interfere with internal hardware | Adjust `snap_offset` and check both mirrored positions, root fillets and insertion travel. |

Changing `wall_thickness` scales several linked features together. For fit tuning, change the relevant individual parameter rather than unintentionally scaling the whole closure.

## ✅ Parameter safeguards and user verification

---

**⚠️ The user must verify parameter combinations after edits.** Creation-script checks do not run automatically when parameters are edited in Fusion. The model has no automatic parameter-validation routine.

Fusion flags features that fail to regenerate. Successful regeneration does **not** establish good printed fit, secure retention, acceptable release force, layer strength or fatigue life.

At a minimum, verify the following:

- Engagement: `0 < lid_gap < lip_radius`. Nominal engagement is `lip_radius - lid_gap`.
- Groove clearance: keep `snap_clearance > 0`; reducing it can cause binding. Groove radius is `lip_radius + snap_clearance`.
- Seam: `lid_seam_gap >= 0`; zero is the intended seated position, not a guarantee of printed preload or no rattle.
- Rim corners: `inside_fillet > lid_gap`, keeping `rim_corner_radius` positive.
- Taper: `0 < snap_thickness < snap_root_thickness`; normally keep `snap_root_thickness = rim_thickness`.
- Flexible length: `0 < rim_depth < lip_depth - 2 * lip_radius`. Root fillets and bead shape further affect usable flexure.
- Tab-to-bottom clearance: conservatively keep `lip_depth < height - wall_thickness - inside_fillet`.
- End clearance: keep tab spans and root-end fillets clear of rounded corners and internal hardware at both mirrored positions.
- Pick entry: `0 < pry_depth < lid_thickness` and `0 < pry_reach < wall_thickness + lid_gap`.
- Notch-to-tab clearance: `length / 2 - pry_width / 2 > snap_offset + snap_length / 2 + snap_root_fillet`; check the mirrored edge too.
- Keep required thicknesses and radii positive. Excessive fillets or chamfers can consume remaining material or fail to regenerate.
- Enlarging the bead also enlarges its groove. Inspect the remaining box-wall thickness at the groove and the actual bead coverage within the tapered tab.

These are geometry guidelines, not a complete set of enforced constraints or a strength guarantee. Inspect feature health, assembled clearances, rim seating, bead engagement and opener access after edits. Internal components require a separate clearance check against the full assembly.

A CAD interference check can identify overlapping solids, but it does not establish printed tolerances, preload, insertion force or release force. Retest the full-size assembly after significant parameter or process changes.
