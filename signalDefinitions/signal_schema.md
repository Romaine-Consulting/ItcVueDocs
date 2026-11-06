# ItcVue Signal Rule Set Guide

ItcVue uses JSON files to define how railroad signal aspects are displayed. Each railroad can have its own file containing one or more rule sets.

## 1. File structure

Create a JSON file named after the railroad (e.g., `BNSF.json`, `UP.json`, or `NS.json`).

```json
{
  "version": "1.0",
  "railroad": "BNSF",
  "ruleSets": {
    "BNSF1": {
      "name": "BNSF-1",
      "aspects": {
        "3": {
          "name": "Clear",
          "appearances": {
            "1": ["G"]
          }
        },
        "6": {
          "name": "Approach Medium",
          "appearances": {
            "1": ["FY"]
          }
        }
      }
    }
  }
}
```

- **railroad:** Railroad abbreviation.
- **ruleSets:** One or more named collections of signal definitions.
- **aspects:** Numeric signal codes and their corresponding names and appearances.
- **appearances:** Signal lamp colors, listed from top to bottom, based on the number of signal heads.

## 2. Signal lamp codes

| Code | Meaning |
|---|---|
| R | Red |
| Y | Yellow |
| G | Green |
| L | Lunar |
| D | Dark |
| FR | Flashing Red |
| FY | Flashing Yellow |
| FG | Flashing Green |

## 3. Defining signal appearances

Each appearance uses a number (`"1"` through `"4"`) indicating the number of signal heads.

For example, Approach Medium:

```json
"6": {
  "name": "Approach Medium",
  "appearances": {
    "1": ["FY"]
  }
}
```

**You do not need to define every possible head count.**

ItcVue automatically adds red indications to the bottom of the largest applicable definition. For example:

- `["G"]` on a three-head signal becomes `["G", "R", "R"]`.
- `["FY"]` on a two-head signal becomes `["FY", "R"]`.
- `["Y", "G"]` on a three-head signal becomes `["Y", "G", "R"]`.
- `["D"]` automatically makes every head dark.

If a signal requires a different appearance for a particular number of heads, add an explicit definition for that head count.

A definition requiring more heads than the physical signal has cannot be used.

## 4. Multiple rule sets

A railroad may use different signal code mappings in different territories. You can define additional rule sets in the same file:

```json
"ruleSets": {
  "BNSF1": {
    "name": "BNSF-1",
    "aspects": {}
  },
  "BNSF2": {
    "name": "BNSF-2",
    "aspects": {}
  }
}
```

Each rule set has its own numeric aspect mappings and signal appearances.

## 5. Sharing your definitions

Use the existing BNSF JSON as a template, replace the railroad information and aspect definitions, and save the completed file as valid JSON.

You can share your completed rule sets with the ItcVue community for inclusion in future updates.