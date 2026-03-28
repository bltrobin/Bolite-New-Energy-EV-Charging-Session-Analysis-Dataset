# Bolite New Energy EV Charging Session Analysis Dataset

This repository contains the public release of the Bolite New Energy EV charging session dataset in Markdown-friendly form. It is a concise, plain-English version of the original PDF summary, aligned with the JSON files that are actually published in this repo.

## Authors

- Ziwei Jeffrey Yang, Bolite New Energy, Shanghai, China
- Yixin Sun, Independent researcher

These authors contributed equally to this work.

## Contribution Note

Haojun Ma contributed to data processing and consolidation during his internship at Bolite New Energy.

## Overview

The dataset includes 720 anonymized EV charging sessions collected from the Bolite New Energy charging network in China. Each session combines:

- Session-level summary metrics
- Battery health indicators
- Temperature-related metrics
- Embedded time-series telemetry for current, voltage, power, and timestamps

This release is suited to battery analytics, charging behavior analysis, thermal studies, and data-driven EV charging research.

## Dataset Snapshot

| Item | Value |
| --- | --- |
| Sessions | 720 |
| Files | 39 JSON files in [`data/`](data/) |
| Time coverage | June 26, 2025 to October 10, 2025 |
| Region | China |
| Rated battery capacity range | 37.5 Ah to 203.0 Ah |
| Charging telemetry | Current, voltage, power, and timestamps |
| Typical sampling interval | About 15 seconds |
| License | CC BY-NC-SA 4.0 |

## Repository Layout

```text
data/
  0000.json
  0001.json
  ...
  0038.json
Field description.md
LICENSE
README.md
```

## Record Format

Each JSON file contains an array of charging session records. All released records share the same 34-field schema.

- Basic session info: `time`, `w`, `m`, `m1`
- Capacity and energy: `a`, `b`, `f`, `g`, `i`, `j`, `k`, `l`, `n`
- State of charge: `o`, `p`, `q`, `q1`
- Time-series telemetry: `c`, `d`, `e`, `h`
- Charge transition markers: `c1`, `c2`, `e1`, `e2`, `h1`
- Battery health: `f1`, `r`, `r1`
- Temperature: `s`, `t`, `u`, `u1`, `x`

The detailed field dictionary is in [`Field description.md`](Field%20description.md).

## Important Data Notes

- The fields `c`, `d`, `e`, and `h` are stored as strings that represent arrays.
- `d` contains Unix timestamps in milliseconds.
- The time-series arrays are aligned by index within each session.
- This README follows the released JSON files. The current public data does not include a `z` field.

## Quick Example

```python
import ast
import json
from pathlib import Path

records = json.loads(Path("data/0000.json").read_text())
session = records[0]

current = ast.literal_eval(session["c"])
timestamps = ast.literal_eval(session["d"])
voltage = ast.literal_eval(session["e"])
power = ast.literal_eval(session["h"])

print(session["time"])
print(session["a"], session["i"], session["o"], session["p"])
print(len(current), len(timestamps), len(voltage), len(power))
```

## Privacy and Usage

The dataset has been anonymized before release. No VINs, GPS coordinates, personal names, license plates, or user identifiers are included in this repository.

Use of the dataset must follow the terms in [`LICENSE`](LICENSE), including:

- Attribution
- Non-commercial use only unless separately authorized
- Share-alike requirements
- No re-identification attempts

## Contact

For questions about the dataset, contact `cwblt@bolite.cc`.
