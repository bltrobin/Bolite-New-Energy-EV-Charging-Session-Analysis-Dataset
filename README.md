# Bolite New Energy: EV Charging Session Analysis Dataset

## Overview
This dataset contains pre-processed and aggregated data from Electric Vehicle (EV) charging sessions collected by Bolite New Energy Co., Ltd. (宝莱特新能源有限公司). Each record represents a complete charging session with both summary metrics and embedded time-series data, providing comprehensive insights into battery performance, charging behavior, and health indicators.

### Dataset Summary
- **Total Charging Sessions**: 720
- **Geographic Region**: China (Bolite New Energy charging network)
- **Battery Capacity Range**: 37.5 - 203 Ah
- **Data Format**: JSON (39 files)
- **Sampling Interval**: ~15 seconds during charging

The data is useful for analyzing:
* **Battery Health Assessment**: Degradation metrics, internal resistance, and capacity retention.
* **Charging Performance**: Energy efficiency, power profiles, and charging curves.
* **Thermal Management**: Temperature evolution and thermal scoring during charging.
* **Quality Metrics**: Multi-dimensional scoring system for battery and charging quality.

## Dataset Organization
The dataset consists of 39 JSON files (`0000.json` through `0038.json`) in the `data/` directory. Each JSON file contains an array of charging session records from multiple vehicles.

### File Structure
```
data/
├── 0000.json
├── 0001.json
├── ...
└── 0038.json
```

Each file contains multiple charging session records in JSON array format.

---

## Data Dictionary

Each charging session record contains the following fields (refer to `字段解释.md` for complete Chinese-English field mappings):

### Basic Session Information

| Field | Unit | Description (Chinese) | Description (English) |
| :--- | :--- | :--- | :--- |
| `time` | DateTime | 充电时间 | Charging session timestamp |
| `w` | Integer | 电池种类 | Battery type |
| `m` | Integer | 电池循环次数 | Battery cycle count |
| `m1` | Integer | 距离上次电池循环差值 | Difference from last battery cycle |

### Capacity and Energy Metrics

| Field | Unit | Description (Chinese) | Description (English) |
| :--- | :--- | :--- | :--- |
| `a` | Ah | 额定容量 | Rated capacity |
| `b` | Ah | 此次充电容量 | Current charging capacity |
| `i` | V | 额定电池电压 | Rated battery voltage |
| `n` | Wh | 额定电池能量 | Rated battery energy |
| `j` | Wh | 现存电池能量 | Current battery energy |
| `g` | Wh | 此次充电能量 | Current charging energy |
| `l` | Wh | 上次充电能量 | Last charging energy |
| `f` | % | 现存容量百分比 | Current capacity percentage |
| `k` | % | 能量保持率 | Energy retention rate |

### State of Charge (SOC)

| Field | Unit | Description (Chinese) | Description (English) |
| :--- | :--- | :--- | :--- |
| `o` | Ratio | SOC开始 | SOC start (0.00 - 1.00) |
| `p` | Ratio | SOC结束 | SOC end (0.00 - 1.00) |
| `q` | Ratio | SOC差 | SOC difference |
| `q1` | Ratio | 最大温差SOC | Maximum temperature difference SOC |

### Time-Series Data (Stored as JSON String Arrays)

These fields contain arrays of measurements taken during the charging session:

| Field | Unit | Description (Chinese) | Description (English) |
| :--- | :--- | :--- | :--- |
| `c` | A | BMS实时电流 | BMS real-time current array |
| `c1` | A | 快充结束电流 | Fast charging end current |
| `c2` | A | 慢充开始电流 | Slow charging start current |
| `d` | ms | BMS记录时间 | BMS recording timestamps (Unix milliseconds) |
| `e` | V | BMS实时电压 | BMS real-time voltage array |
| `e1` | V | 快充结束电压 | Fast charging end voltage |
| `e2` | V | 慢充开始电压 | Slow charging start voltage |
| `h` | W | BMS实时充电功率 | BMS real-time charging power array |
| `h1` | W | BMS平均充电功率 | BMS average charging power |

### Battery Health Indicators

| Field | Unit | Description (Chinese) | Description (English) |
| :--- | :--- | :--- | :--- |
| `f1` | Ω | 电池内部电阻 | Battery internal resistance |
| `r` | - | 此次衰败每循环 | Current degradation per cycle |
| `r1` | - | 平均衰败每循环 | Average degradation per cycle |

### Temperature Metrics

| Field | Unit | Description (Chinese) | Description (English) |
| :--- | :--- | :--- | :--- |
| `s` | °C | 充电开始温度 | Charging start temperature |
| `t` | °C | 充电结束温度 | Charging end temperature |
| `u` | °C/% | 升温/1%充电 | Temperature rise per 1% charge |
| `u1` | °C/% | 归一升温/1%充电 | Normalized temperature rise per 1% charge |
| `x` | °C | 温度差可接受值 | Acceptable temperature difference |

### Voltage Stability

| Field | Unit | Description (Chinese) | Description (English) |
| :--- | :--- | :--- | :--- |
| `z` | - | voltage vs. time负曲率 | Voltage vs. time negative curvature |

---

## Usage Example
The following example demonstrates how to load and analyze the charging session data using Python.

```python
import json
import numpy as np
import pandas as pd

# 1. Load a JSON file containing charging sessions
with open('data/0000.json', 'r') as f:
    sessions = json.load(f)

# 2. Access the first charging session
session = sessions[0]

# 3. Display basic session information
print(f"Charging Time: {session['time']}")
print(f"Rated Capacity: {session['a']} Ah")
print(f"Rated Voltage: {session['i']} V")
print(f"Battery Cycle Count: {session['m']}")
print(f"SOC: {session['o']*100:.1f}% → {session['p']*100:.1f}%")

# 4. Parse time-series data from JSON string arrays
import ast

# Current measurements (A)
current_array = ast.literal_eval(session['c'])
# Voltage measurements (V)
voltage_array = ast.literal_eval(session['e'])
# Timestamps (Unix milliseconds)
timestamp_array = ast.literal_eval(session['d'])
# Power measurements (W)
power_array = ast.literal_eval(session['h'])

# 5. Create a time-series DataFrame
df_timeseries = pd.DataFrame({
    'timestamp': pd.to_datetime(timestamp_array, unit='ms'),
    'current_A': current_array,
    'voltage_V': voltage_array,
    'power_W': power_array
})

print(f"\nTime-series data points: {len(df_timeseries)}")
print(f"Charging duration: {(df_timeseries['timestamp'].iloc[-1] - df_timeseries['timestamp'].iloc[0]).total_seconds()/60:.1f} minutes")
print(f"Average power: {session['h1']:.2f} W")
print(f"Peak power: {max(power_array):.2f} W")

# 6. Analyze battery health metrics
print(f"\nBattery Health:")
print(f"  Internal Resistance: {session['f1']:.4f} Ω")
print(f"  Energy Retention: {session['k']:.2f}%")
print(f"  Avg Degradation/Cycle: {session['r1']:.4f}")
print(f"  Capacity: {session['f']:.2f}%")
```

## Data Format Notes

### Time-Series Arrays
The fields `c`, `d`, `e`, and `h` contain time-series data stored as JSON string arrays. To use them:
- Parse using `ast.literal_eval()` or `json.loads()` in Python
- `d` contains Unix timestamps in milliseconds
- Arrays are synchronized by index (same length)

---

## Privacy and Data Usage

### Data Anonymization
This dataset has been **fully anonymized** to protect user privacy:
- All vehicle identification numbers (VINs) have been removed
- No GPS coordinates or charging station locations are included
- No personal information (names, license plates, user IDs) is present
- Data is aggregated from multiple vehicles across the charging network
- Timestamps are preserved for temporal analysis but cannot be linked to specific individuals

### Intended Use
This dataset is intended for:
- Battery health and degradation research
- Charging optimization algorithm development
- Machine learning model training and validation
- Academic research and education
- EV infrastructure planning and analysis

### Prohibited Uses
Users of this dataset must NOT:
- Attempt to re-identify individual vehicles, users, or locations
- Combine with other datasets to de-anonymize individuals
- Use for surveillance or tracking purposes
- Use for commercial purposes without prior written permission from Bolite New Energy Co., Ltd.

**Note**: See the `LICENSE` file for complete terms and conditions.

---

## Dataset Background

### Data Collection
**Source**: Bolite New Energy Co., Ltd. (宝莱特新能源有限公司) DC fast charging network infrastructure

**Collection Method**:
- Data extracted from Battery Management System (BMS) via standardized charging protocols
- Real-time measurements captured during charging sessions
- Automated data validation and quality checks applied
- Time-series data sampled at approximately 15-second intervals

**Infrastructure**:
- DC fast charging stations (high-power charging)
- Charging power range: 30-250 kW (estimated from power measurements)
- Compatible with mainstream EV models in Chinese market

### Sample Characteristics
- Mixed fleet of electric vehicles with varying battery capacities
- Batteries at various lifecycle stages (cycle counts from 1 to hundreds)
- Real-world charging behavior including fast and slow charging transitions

### Quality Metrics
- All 720 sessions contain complete basic information
- Time-series data length varies by session duration (typically 100-200 data points)
- Charging duration ranges from ~10 minutes to several hours
- Data completeness: >95% for all required fields

### Known Limitations
- Ambient temperature and weather conditions not included
- Scoring algorithms are proprietary and not fully disclosed
- Vehicle make/model information not provided to preserve anonymity

---

## Additional Resources
- **Field Descriptions**: See `字段解释.md` for complete Chinese-English field mappings
- **Data Files**: 39 JSON files in the `data/` directory
- **License**: See `LICENSE` file for terms of use

---

## License
This dataset is released under the **Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International License (CC BY-NC-SA 4.0)**.

See the `LICENSE` file for full terms.

**Summary**:
- Free to use for academic and non-commercial research
- Must provide attribution
- Adaptations must be shared under the same license
- Commercial use requires written permission from Bolite New Energy Co., Ltd.
- No re-identification attempts allowed

For commercial licensing inquiries, please contact: Bolite New Energy Co., Ltd.

---

## Citation
If you use this dataset in your research, please cite:

```bibtex
@misc{bolite_charging_dataset_2025,
  title = {Bolite New Energy EV Charging Session Analysis Dataset},
  author = {{Bolite New Energy Co., Ltd.}},
  year = {2025},
  month = {June-October},
  note = {720 anonymized EV charging sessions with battery health metrics},
  howpublished = {Dataset repository},
  license = {CC BY-NC-SA 4.0}
}
```

**Suggested acknowledgment text**:
> "This research uses data from the Bolite New Energy EV Charging Session Analysis Dataset, which contains 720 anonymized charging sessions collected from June to October 2025."

---

## Contact
For questions, commercial use inquiries, or collaboration opportunities:
- **Organization**: Bolite New Energy Co., Ltd. (宝莱特新能源有限公司)
- **Dataset Issues**: Please open an issue in this repository
- **Commercial Licensing**: http://m.bolite.cc/
