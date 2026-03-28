# BLT Dataset Field Description

This file documents the 34 fields used in the released charging session records.

## Session and History

| Field | Meaning | Type | Unit / Format |
| --- | --- | --- | --- |
| `time` | Charging session timestamp | string | `YYYY-MM-DD HH:MM:SS` |
| `w` | Battery type code | float | code |
| `m` | Battery cycle count | float | cycles |
| `m1` | Difference from the last battery cycle reference | float | cycles |

## Capacity and Energy

| Field | Meaning | Type | Unit / Format |
| --- | --- | --- | --- |
| `a` | Rated capacity | float | Ah |
| `b` | Charging capacity in this session | float | Ah |
| `f` | Current capacity percentage | float | % |
| `g` | Charging energy in this session | float | Wh |
| `i` | Rated battery voltage | float | V |
| `j` | Current battery energy | float | Wh |
| `k` | Energy retention rate | float | % |
| `l` | Charging energy in the previous session | float | Wh |
| `n` | Rated battery energy | float | Wh |

## State of Charge

| Field | Meaning | Type | Unit / Format |
| --- | --- | --- | --- |
| `o` | SOC at session start | float | ratio |
| `p` | SOC at session end | float | ratio |
| `q` | SOC increase during the session | float | ratio |
| `q1` | SOC at the point of maximum temperature difference | float | ratio |

## Time-Series Telemetry

These fields are stored as strings that represent arrays.

| Field | Meaning | Type | Unit / Format |
| --- | --- | --- | --- |
| `c` | BMS real-time current series | string | array of A values |
| `d` | BMS timestamp series | string | array of Unix ms values |
| `e` | BMS real-time voltage series | string | array of V values |
| `h` | BMS real-time charging power series | string | array of W values |

## Charge Transition Markers

| Field | Meaning | Type | Unit / Format |
| --- | --- | --- | --- |
| `c1` | Current at fast-charging end | float | A |
| `c2` | Current at slow-charging start | float | A |
| `e1` | Voltage at fast-charging end | float | V |
| `e2` | Voltage at slow-charging start | float | V |
| `h1` | Average charging power across the session | float | W |

## Battery Health

| Field | Meaning | Type | Unit / Format |
| --- | --- | --- | --- |
| `f1` | Battery internal resistance | float | ohm |
| `r` | Degradation per cycle for this session | float | derived value |
| `r1` | Average degradation per cycle | float | derived value |

## Temperature

| Field | Meaning | Type | Unit / Format |
| --- | --- | --- | --- |
| `s` | Temperature at charge start | float | degC |
| `t` | Temperature at charge end | float | degC |
| `u` | Temperature rise per 1% charge | float | degC per % |
| `u1` | Normalized temperature rise per 1% charge | float | derived value |
| `x` | Acceptable temperature difference | float | derived value |
