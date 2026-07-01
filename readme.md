# Layout Guide for Beta Testers

This guide explains how to build a new route layout by editing the active layout file in `Server/Data`.

Use the separate reference layout in that same folder as a working example of supported patterns. Do not edit the reference layout in place. Start a fresh layout in the active file, then copy and adapt small pieces from the reference when you need an example.

This guide only covers patterns already shown in the current reference layout.

## Start the App

Beta users receive a built package. You do not need source code tools, Node, npm, Visual Studio, or the .NET SDK to edit and test a layout.

To start the app:

1. Open the package folder.
2. Click `Server.exe`.
3. Open the browser page at `http://localhost:576`.

The layout file you will edit is:

```text
Server/Data/currentLayout.json
```

The app loads that file when the browser asks for the layout. After you change and save `currentLayout.json`, refresh the browser page. You do not need to restart `Server.exe` for normal layout edits.

There is also a railroad configuration file:

```text
Server/Data/railroadConfig.json
```

Most layout work is done in `currentLayout.json`. Use `railroadConfig.json` only when the packet order for a railroad needs to be configured.

## Read the File from Top to Bottom

The layout file has three main parts: overall layout settings, reusable templates, and placed layout items.

### Overall Layout Settings

At the top of the file you will see settings like these:

```json
{
  "id": "UP Moffatt Tunnel Subdivision East",
  "canvasWidth": 82,
  "canvasHeight": 61,
  "showGridLines": false,
  "signalStopValues": [14, 15, 30]
}
```

These describe the whole drawing area.

- `id` is the layout name.
- `canvasWidth` is the number of grid spaces across.
- `canvasHeight` is the number of grid spaces tall.
- `showGridLines` shows or hides the grid.
- `signalStopValues` lists the raw signal values that should display as red. Other demonstrated signal values display as green.

Turning grid lines on can help while editing. When grid lines are on, right-clicking the layout shows the grid coordinate. If you right-click inside a template, it also shows that spot's position relative to the template.

### Reusable Templates

The `templates` section contains reusable control point patterns. A template is a small drawing made from track, switch, signal, connector, crossing, and label pieces.

The reference layout demonstrates simple standard patterns such as:

- `standard-left-up`
- `standard-left-down`
- `standard-right-up`
- `standard-right-down`
- `block-signal-pair`

These are the best starting points for most users. They are small, predictable, and easy to place more than once.

The reference layout also includes larger custom examples such as:

- `arvada`
- `c&s`
- `pecos`
- `broadway`
- `utah`

Use those as advanced references when you need a larger custom arrangement. They are useful examples, but they are not the best first step for a new layout.

### Placed Layout Items

The `items` section is the actual visible layout. This is where templates and connecting pieces are placed on the grid.

For normal layout building, place these directly in `items`:

- `template-instance` for control points and signal pairs
- `track` for straight connecting runs
- `portal` for jumps from one row to another
- `diagonal` and `angled` for short connector pieces
- `label` for large standalone titles or railroad names

The reference layout also shows raw `signal`, `switch-point`, `crossing-45`, and many smaller labels inside templates. Most users should keep those inside templates so the pieces move together and keep their live mappings.

## Build a New Location

Use a copy-and-adapt workflow.

1. Start with a fresh `currentLayout.json`.
2. Keep the reference layout open as a read-only example.
3. Pick the simplest reference pattern that matches your location.
4. Copy one placed `template-instance` into your new `items` list.
5. Change its `id`, position, visible text, and ItcMon mapping.
6. Add straight `track` pieces to connect it to the next location.
7. Save the file and refresh the browser.

For a normal single-switch control point, start from one of the standard templates. For a simple pair of block signals, start from `block-signal-pair`.

A placed template now looks like this:

```json
{
  "id": "780221903503",
  "kind": "template-instance",
  "template": "standard-left-up",
  "x": 10,
  "y": 48,
  "wiu": "780221903503",
  "signalIndexes": {
    "sig-st": 3,
    "sig-t1": 1,
    "sig-t2": 2
  },
  "switchIndexes": {
    "switch1": 1
  },
  "labelTexts": {
    "label-name": "W East Portal",
    "label-ds": "DS051"
  }
}
```

When you adapt it:

- `id` must be unique in the file.
- `template` chooses the reusable pattern.
- `x` and `y` place the pattern on the grid.
- `wiu` is the control point ID, meaning the ItcMon device this location listens to.
- `signalIndexes` and `switchIndexes` connect template roles to live ItcMon values.
- `labelTexts` fills in the visible text roles provided by the template.

Older examples may use `label` and `subLabel`. The current reference layout uses `labelTexts`, usually with `label-name` for the location name and `label-ds` for the DS number.

## Place Connecting Items

After you place control points with `template-instance`, use direct `items` entries to connect them. This is normal layout assembly, not custom template work.

Place these items directly in the `items` section when you need them:

- `track` for straight runs between control points
- `portal` for jumps from one row to another
- `diagonal` and `angled` for short connector pieces
- `label` for standalone titles, authorship text, or railroad names

### Straight Track Runs

Use `track` items for long straight sections between control points.

```json
{ "kind": "track", "id": "main-1", "x": 15, "y": 48, "length": 6 }
```

Useful fields:

- `id` is the unique name for this item.
- `x` and `y` place the left end on the grid.
- `length` controls how many grid spaces the track covers.
- `blockEndLeft` and `blockEndRight` can hide the end bar when a track touches another piece.

### Portals

Portals let a route continue somewhere else, usually on another row.

```json
{ "kind": "portal", "id": "portal-b-right", "x": 81, "y": 48, "pairId": "B" }
```

Each portal needs a matching partner with the same `pairId`. If one portal uses `"pairId": "B"`, the other portal in that pair must also use `"pairId": "B"`.

### Diagonal and Angled Connectors

The reference layout uses `diagonal` and `angled` pieces for short connections between straight track runs.

Useful fields:

- `x` and `y` place the connector on the grid.
- `flip` mirrors a diagonal connector.
- `rotation` turns an angled connector. Use the values demonstrated in the reference layout.

Use these pieces sparingly. If a standard template already includes the shape you need, place that template first and connect to it with straight track.

### Standalone Labels

Direct labels are useful for large titles and railroad names. For normal control point labels, prefer template labels and `labelTexts`.

```json
{
  "kind": "label",
  "x": 41,
  "y": 58,
  "text": "UP Moffat Tunnel Subdivision - East",
  "size": "xl",
  "offsetY": 0.5,
  "color": "yellow"
}
```

Useful fields:

- `text` is what appears on the screen.
- `size` can use the demonstrated values `sm`, `md`, `lg`, or `xl`.
- `color` can use demonstrated values such as `yellow` or `orange`; leave it out for the normal white label color.
- `offsetX` and `offsetY` nudge the label without changing its grid anchor.

Position labels carefully. Labels do not affect routing, but they can visually cover nearby track, signals, or portals.

## Build a Custom Template

Build or adapt a template when the normal placed items are not enough. A custom template is useful when one location has several signals, switches, labels, crossings, or connector pieces that should move together as one control point.

For most new routes, do not start here. Begin with the standard templates and direct connecting items first. Study custom templates only when your location needs a larger arrangement that the simple patterns cannot describe.

Inside a template, each piece has:

- `kind`, which says what the piece is
- `role`, which gives the piece a name inside the template
- `x` and `y`, which place the piece relative to the template's starting point

Template roles matter because the placed `template-instance` uses those names later in `signalIndexes`, `switchIndexes`, and `labelTexts`.

For example, if a template contains a switch with:

```json
{ "kind": "switch-point", "role": "switch1", "x": 1, "y": 0 }
```

Then the placed template can map it like this:

```json
"switchIndexes": { "switch1": 1 }
```

Signals inside templates should sit next to the track they control. In demonstrated templates, a signal uses `trackRole` to name the track role it controls:

```json
{
  "kind": "signal",
  "role": "sig-t1",
  "x": 4,
  "y": 1,
  "facing": "left",
  "trackRole": "track-1"
}
```

That lets the app connect the visible signal to the right track after the template is placed.

Labels inside templates can either have fixed text or be filled by `labelTexts` in the placed template. The standard templates use roles such as `label-name` and `label-ds` so each placed location can provide its own text.

```json
{
  "kind": "label",
  "role": "label-name",
  "x": 2,
  "y": 5,
  "size": "md",
  "offsetY": 0.5
}
```

The placed template fills it like this:

```json
"labelTexts": {
  "label-name": "E Tolland",
  "label-ds": "DS047"
}
```

### Larger Custom Control Points

For larger control points, study the advanced reference templates after you are comfortable with the simple ones.

- `arvada` shows one route fanning into several tracks.
- `c&s` shows a larger junction with many switch and signal roles.
- `pecos` shows a compact multi-track section.
- `broadway` shows another multi-track arrangement with standard signals and labels.
- `utah` shows a larger junction that includes `crossing-45` pieces.

Copying these requires more care because there are more roles to map and more places where a small coordinate mistake can look like a signal or route problem. Use them as references for how to structure a template, not as the first pattern for most new locations.

### 45-degree Crossings

The demonstrated `crossing-45` pieces are inside the `utah` template. Treat them as advanced template pieces, not normal direct placements.

Useful fields shown in the reference:

- `diagonalDelta` tells the route how the diagonal path changes row as it crosses.
- `diagonalFlip` mirrors the diagonal part of the crossing.

When copying a crossing pattern, copy the nearby track and connector pieces with it. A crossing is not just a drawing; it also affects route highlighting.

## Map Your ItcMon Values

Templates are not just drawings. They also provide named roles that connect visible pieces to live ItcMon values.

Inside a template, each important piece has a `role`, such as `switch1`, `sig-t1`, or `track-2`. When you place that template, the `signalIndexes` and `switchIndexes` sections say which live ItcMon value belongs to each role.

### Control Point ID

The control point ID is the ItcMon device ID for the layout section.

In the demonstrated layout file, this field is named `wiu`:

```json
"wiu": "780221903503"
```

If this value is wrong, the drawing may look broken, stale, or unresponsive even though the coordinates are fine. The section is simply listening to the wrong device.

### Signal Indexes

`signalIndexes` maps template signal roles to signal positions from ItcMon.

```json
"signalIndexes": { "sig-st": 3, "sig-t1": 1, "sig-t2": 2 }
```

This means:

- `sig-st` uses the third signal from ItcMon.
- `sig-t1` uses the first signal from ItcMon.
- `sig-t2` uses the second signal from ItcMon.

The numbers you enter are 1-based. `1` means first, `2` means second, and `3` means third. The app handles the internal conversion automatically.

### Switch Indexes

`switchIndexes` maps template switch roles to switch positions from ItcMon.

```json
"switchIndexes": { "switch1": 1, "switch2": 2 }
```

This means:

- `switch1` uses the first switch from ItcMon.
- `switch2` uses the second switch from ItcMon.

These numbers are also 1-based. Enter them the way an operator or tester would count them from ItcMon data.

### Unmapped Values

If a template role should not be connected to live data, leave it out or use `-1`.

```json
"signalIndexes": {
  "sig-t1-left": -1,
  "sig-t2-left": 6
}
```

The `-1` value means that part is intentionally not connected. This is useful when a larger template includes a signal or switch position that your route does not use.

### Signal Stop Values

`signalStopValues` controls which raw signal values count as red. The current reference layout uses:

```json
"signalStopValues": [14, 15, 30]
```

If signals appear green when they should be red, or red when they should be green, check this list along with the signal index order.

### Railroad Packet Order

The client uses `Server/Data/railroadConfig.json` to know whether a railroad sends switch data before signal data. The railroad code comes from the WIU ID.

Most beta testers should not need to change this file during layout drawing. If packets from a railroad decode incorrectly across many locations, check the matching railroad entry before changing every signal or switch index by hand.

## Practical Layout Rules

- Signals are wayside devices. Place them next to the track they control.
- A signal inside a template should use `trackRole` so it attaches to the correct track role.
- Portal pairs need matching `pairId` values.
- Route behavior depends on grid positions, not JSON item order.
- Routes stop at blocking signals and at switches that do not connect for the current switch position.
- Switches from different WIUs can stop route propagation.
- Bad WIU IDs, signal mappings, switch mappings, or stop values often look like display problems.

## Common Mistakes

- Editing the reference layout instead of using it as an example.
- Copying an item but forgetting to change its `id`.
- Moving a template without moving the connecting `track` pieces.
- Pairing only one portal, or giving the two portals different `pairId` values.
- Expecting JSON order to fix route behavior. Routes follow grid positions, not item order.
- Using the wrong control point ID in `wiu`.
- Mapping signals or switches in the wrong order.
- Forgetting that signal and switch mappings are entered as 1-based numbers.
- Treating an intentional `-1` as an error, or accidentally leaving a needed role unmapped.
- Using old `label` and `subLabel` fields instead of current `labelTexts`.
- Placing labels where they overlap signals, portals, or nearby tracks.

Bad device IDs or mapping values often look like display problems. Before moving track pieces around, check the WIU ID, signal order, switch order, and `signalStopValues`.

## Final Checklist

Before handing off a new or changed layout, confirm:

- Every item `id` is unique where an `id` is used.
- Coordinates line up cleanly on the grid.
- Straight tracks connect to the intended template tracks.
- Signals sit next to the correct tracks.
- Signal template pieces point to the correct `trackRole`.
- Each portal has a matching partner with the same `pairId`.
- Each control point ID in `wiu` matches the intended ItcMon device.
- Signal index numbers match the real ItcMon signal order.
- Switch index numbers match the real ItcMon switch order.
- Unmapped values are intentional, either omitted or set to `-1`.
- `labelTexts` keys match label roles in the template.
- `signalStopValues` matches the signal values that should show red.
- Railroad packet order is correct in `railroadConfig.json` if decode behavior is wrong across a railroad.
- Labels are positioned carefully and do not cover nearby layout pieces.

Start simple, refresh often, and use the reference layout as a catalog of proven examples rather than as the file you edit.
