# Tool Pocket Generator V149

## Provenance
- Built directly from `Tool_Pocket_Generator_V148_FINAL.zip`.
- V148 remains the immediate source; no other build was used as the starting point.

## Problem addressed
Changing the orange workspace rectangle after a photo outline was detected or a JSON profile was imported changed the canvas frame scale/origin, while the stored point coordinates remained in the old canvas frame. The points could therefore be interpreted as a different physical outline when converted back to millimetres.

## Change in V149 — one functional scope
- When **Detect Rectangle in Workspace** changes the workspace dimensions, existing profile points are first converted to their current millimetre coordinates, then mapped into the new canvas frame.
- This preserves the physical point positions and outline shape instead of leaving stale screen coordinates behind.
- Curve control references are synchronized after the points are remapped.
- The workspace rectangle remains a display/work-area setting and does not resize the physical tool outline.

## Required workflows to retest
1. **New Tool + photo:** enter different workspace dimensions, load the same tool photo, confirm the physical grid, then detect the outline.
2. **JSON imported, rectangle unchanged:** import `Tool_Name_V75_Profile(2).json`, keep the rectangle dimensions, confirm the physical grid, and detect the outline.
3. **JSON imported, rectangle changed:** import the same JSON, change the workspace Length × Width, press **Detect Rectangle in Workspace**, then confirm the physical grid and detect the outline.
4. Compare the detected outline's physical dimensions and shape across the cases. Workspace changes must not alter physical geometry. Confirm the outline stays visible and edit points as usual.
5. Check representative Straight and Curve points remain attached to the profile and Curve/Straight switching and dragging behave as before.

## What was not changed
- Photo silhouette extraction or physical-ruler measurement algorithm.
- `detectPhoto()` mapping logic.
- Curve sampling, arc construction, drawing path, point list, point insertion/deletion, or drag event wiring.
- JSON import data interpretation, dimensions-from-grid step, Close Profile, Geometry Check, STL geometry/export, tray settings, or unrelated UI.

## Verification performed
- Source change limited to `detectWorkspaceRectangle()` plus the V149 version label in the saved-tool metadata and heading.
- JavaScript syntax validation with Node.js.
- ZIP archive integrity validation.
- Numeric frame-remapping test across different workspace sizes: the point coordinates in millimetres are invariant to the remap.
- Source comparison confirms Curve sampling, profile drawing, point dragging, and Curve/Straight point-type logic are unchanged from V148.

## Device-test limitation
The app was not interactively run in iPad Safari in this environment. The three photo/JSON workflows and visual dot behaviour still require your iPad retest. This is a source-verified candidate, not a claim that the iPad test has passed.
