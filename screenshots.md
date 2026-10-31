# Screenshot Checklist

Capture the current interface or source tool described in each item. Keep the same application window size and browser zoom for captures from the same app. Crop unrelated desktop area, but retain enough interface to identify the control. Redact account names, WIU identifiers, machine names, network addresses, and credentials. Never include secrets. Save to the listed path and remove a guide placeholder only after its image exists.

The guides remain usable without screenshots. Preserve existing images; placeholders are descriptions, not broken image embeds.

All Layout Builder captures listed below are complete and embedded. Unavailable builder-entry and copy/paste captures have been removed from the checklist; the guide instructions remain.

## Quick Start

All five Quick Start captures are complete and embedded in `quick-start.md`.

| ID | Caption | Expected state | Image file |
| --- | --- | --- | --- |
| QS-01 | Download the ItcVue installer | Website **Downloads** page with Windows download button; private account details hidden. | `images/quick-start/qs-01-download.png` |
| QS-02 | Desktop connection status | Connections page showing **ItcVue Aggregator**, **MCR**, and **ZMQ / IGW** options and status. | `images/quick-start/qs-02-connection-status.png` |
| QS-03A | Download a layout | Website **Layouts** catalog download action. | `images/quick-start/qs-03a-download-layout.png` |
| QS-03B | Import a layout | Desktop **Layouts (Local)** import control with `.itcvl` file selection. | `images/quick-start/qs-03b-import-layout.png` |
| QS-07 | Moffat layout on first launch | **UP Moffat Tunnel Subdivision East** in the monitor on first launch. | `images/quick-start/qs-07-moffat-first-view.png` |

## Layout Builder

| ID | Caption | Captured state | Image file |
| --- | --- | --- | --- |
| LB-02 | Create a blank layout | **New Layout** dialog with name, width, and height fields. | `images/layout-builder/lb-01-builder-controls.png` |
| LB-03 | Builder and instance overview | Full builder showing an assembly instance inspector with WIU, packet counts, and signal mappings. | `images/layout-builder/lb-03-builder-overview.png` |
| LB-05 | Assembly Editor | Custom assembly editor with a selected signal and its inspector. | `images/layout-builder/lb-05-assembly-editor.png` |
| LB-07 | Instance label options | Instance name and milepost fields, offsets, and label hover controls. | `images/layout-builder/lb-07-label-inspector.png` |
| LB-08 | Grid and layout settings | Canvas and layout inspector showing grid settings. | `images/layout-builder/lb-08-grid-settings.png` |
| LB-09 | Additional instance options | Remaining instance name, milepost, and label behavior options. | `images/layout-builder/Template Instance Options Extended.png` |

## Layout Design

| ID | Caption | Captured state | Image file |
| --- | --- | --- | --- |
| LD-01 | Moffat and Glenwood boundary context | Moffat Tunnel Subdivision layout extending into Glenwood Springs Subdivision, with the boundary labeled. | `images/layout-design/ld-01-monitor-territory.png` |
| LD-02 | Compact territory rows and portals | BNSF Brush monitor view in four compact rows with labeled portals. | `images/layout-design/ld-02-width-and-orientation.png` |
| LD-03A | Compact assembly definition | Compact crossover assembly open in the Assembly Editor. | `images/layout-design/ld-03a-assembly-editor.png` |
| LD-03B | Repeated assembly instances | Compact assemblies repeated at Carroll, Winston, Ben Hill, and Lithia Springs. | `images/layout-design/ld-03b-repeated-instances.png` |
| LD-06 | Signal and label conventions | Crews and Kelker opposing signals, visible 1- and 2-mileposts, and compact intermediate signals. | `images/layout-design/ld-06-signal-and-label-conventions.png` |
| LD-12 | Directional intermediate signal arrangement | Custom single-signal assembly with short track and label; Track role inspector is not shown. | `images/layout-design/directional-workaround.png` |

## WIU Mapping and Troubleshooting

| ID | Caption | Expected state | Planned file |
| --- | --- | --- | --- |
| LD-04 | WIU counts and role mappings | Instance inspector with WIU ID, total counts, and role mappings. | `images/layout-design/ld-04-packet-mapping.png` |
| LD-05A | Decode-warning label | Steady bright-magenta monitor label, distinct from static magenta text. | `images/layout-design/ld-05a-decode-warning.png` |
| LD-05B | Builder overlap | Magenta builder cell marking overlapping track-like geometry. | `images/layout-design/ld-05b-builder-overlap.png` |
| LD-05C | Invalid switch code | Switch indicator showing magenta for received code `11`. | `images/layout-design/ld-05c-invalid-switch.png` |
| LD-07 | Paired block-signal behavior | Pair instance with two green signals and no outgoing highlights originating from them. | `images/layout-design/ld-07-block-signal-pair.png` |
| LD-08A | Unassigned signal track role | Custom single-signal assembly with `Track role` cleared in editor. | `images/layout-design/ld-08a-unassigned-role.png` |
| LD-08B | Unassigned signal monitor behavior | Monitor view showing the signal does not originate a route. | `images/layout-design/ld-08b-unassigned-monitor.png` |
| LD-10 | Packet field details | WIU Details with ID, available packet timestamp, source, raw bits, field boundaries, and decoded sections. | `images/layout-design/ld-10-wiu-packet-details.png` |

## IGW Setup

| ID | Caption | Expected state | Planned file |
| --- | --- | --- | --- |
| IGW-01 | MCR channel discovery | Terminal with `mcr -a` showing active feeds; redact identifying machine details. | `images/igw-setup/igw-01-mcr-active-channels.png` |
| IGW-02 | IGW configuration precedence | `local/igw.json` in program directory or startup output showing loaded config path. | `images/igw-setup/igw-02-local-config.png` |
| IGW-03 | IGW startup endpoint | Startup output with loaded config path and publication endpoint; omit secrets. | `images/igw-setup/igw-03-igw-startup.png` |
| IGW-04 | Local ItcVue connection status | Connections view with direct MCR and Aggregator disabled and ZMQ / IGW enabled. | `images/igw-setup/igw-04-local-connections.png` |
