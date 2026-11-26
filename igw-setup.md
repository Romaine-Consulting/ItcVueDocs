# IGW Setup

Use IGW (ITCMON Gateway) to aggregate local WIU status from MCR/s2p and pitch it to an upstream aggregator such as ItcVue. As long as **MCR** and **IGW** are running with matching channel coverage, you should be pitching data. For install and monitor basics see **Quick Start**. Official IGW behavior and config sections are described in [ITCMON routing notes](https://github.com/katsojuna/itcmon/blob/main/doc/6-routing.txt).

## Prerequisites

- ITCMON installed, with MCR receiving the RF channels active in your area.
- An ItcVue account that can create an IGW upload key (aggregator destination and credential).

IGW reads `local/igw.json` first, then `./igw.json`. Prefer `local/` so upgrades do not overwrite your config. Validate JSON syntax before relying on a quiet failure to start.

## Create the aggregator credential

In the ItcVue web app, create an IGW API key and download or copy the JSON blob it provides. That blob is typically the `zjout` section: host, port, and key for the upstream aggregator.

Do not commit keys into docs or public repos. Treat the key like a password.

## Install `local/igw.json`

Copy the blob into your ITCMON `local` folder as `igw.json`.

A complete leaf-site config needs both:

| Section | Role |
| --- | --- |
| `servers` | TCP connections to local s2p ports that MCR started for each enabled channel/rate |
| `zjout` | Outgoing connection to the aggregator using the ItcVue key |

The web-app blob alone may supply only `zjout`. If so, keep that `zjout` and add a `servers` list for every channel MCR is actually receiving.

Example shape (placeholder key; use your own host/port/key from ItcVue):

```json
{
  "servers": [
    { "name": "PC 125 HR", "host": "127.0.0.1", "port": 20125 },
    { "name": "PC 125 FR", "host": "127.0.0.1", "port": 18125 }
  ],
  "zjout": {
    "servers": [
      {
        "host": "aggy.example.com",
        "port": 5555,
        "key": "YOUR_KEY_FROM_ITCVUE"
      }
    ]
  }
}
```

s2p port convention used by ITCMON samples: half rate (HR) is `20` + channel number; full rate (FR) is `18` + channel number (for example channel 125 → `20125` HR, `18125` FR). Host is usually `127.0.0.1` when MCR runs on the same machine.

Optional `zjpub` and `zjin` sections are for local consumers or accepting uploads from other IGW nodes. A site that only pitches upstream can omit them. See the [routing notes](https://github.com/katsojuna/itcmon/blob/main/doc/6-routing.txt) for details.

## Match channels: Servers tab, MCR, and IGW

Make sure that neither `mcr.json` or `igw.json` omit channels you care about. Confirm coverage before assuming silence is an aggregator problem.

1. In ITCMON, open the **Servers** tab. Columns are **Name**, **IP**, **Port**, **Channel**, **Enabled**, and **Packets**. The tell for live traffic is a non-empty **Packets** count—note every **Channel** that is accumulating packets.
2. In `mcr.json`, ensure each of those channels has the needed HR and/or FR entries. `channels.json` only maps channel numbers to frequencies; it does not enable receive.
3. In `igw.json` `servers`, list a matching s2p entry for each enabled MCR channel/rate (same host/port pattern as the Servers tab). A channel heard by MCR but missing from IGW `servers` is not pitched.

| Active on Servers (Packets) | In `mcr.json` enabled? | In `igw.json` `servers`? | Result |
| --- | --- | --- | --- |
| yes | yes | yes | pitched |
| yes | no | — | not heard |
| yes | yes | no | heard locally, not pitched |

A practical starting `servers` list is the set of channels you already enable in `mcr.json` (or the sample list shipped with ITCMON for the national WIU channels), trimmed or extended to match rows that show **Packets**.

## Run IGW

With MCR already running, start IGW from the ITCMON directory:

```text
igw.exe -D
```

`-D` prints debug output. Confirm successful transfers to the aggregator, then stop and restart without `-D` for normal operation:

```text
igw.exe
```

If nothing is pitched, re-check that `servers` is present and non-empty, that those ports match live s2p processes from MCR, and that `zjout` uses the current ItcVue key.

## Use the data in ItcVue

Once IGW is pitching, layouts that rely on the ItcVue Aggregator receive the uploaded WIU status through that path. For a direct local feed instead, see **Quick Start** (Connections: **MCR** or **ZMQ / IGW**).
