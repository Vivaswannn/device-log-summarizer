# Device Log Summarizer CLI

A small Python command-line project for the Software Engineering Intern technical assessment. It covers sensor-value cleaning, CSV aggregation, JSON export, terminal reporting, and AI-assisted chart generation.

> **Sample-data note:** The assessment brief names `device_telemetry.csv` but does not include its rows. The CSV in this project is illustrative sample data. Replace it with the supplied assessment CSV if one is provided separately.

## Requirements

- Python 3.10 or newer
- `matplotlib` (listed in `requirements.txt`) for chart generation

The summarizer and tests use only the Python standard library.

## Setup

From this project directory:

```bash
python -m pip install -r requirements.txt
```

## Section 1 - Clean sensor readings

The `clean_sensor_readings()` function in `src/sanitizer.py`:

- converts values to floats where possible;
- drops `None`, non-numeric values, booleans, non-finite values, and negative values;
- clips values above `500.0` to `500.0`;
- returns an empty list for an empty or `None` input.

Example:

```python
from src.sanitizer import clean_sensor_readings

print(clean_sensor_readings([10, "20.5", None, -3, "bad", 700]))
# [10.0, 20.5, 500.0]
```

## Section 2 - Aggregate telemetry CSV

Run from the project directory:

```bash
python -m src.summarize \
  --input device_telemetry.csv \
  --output output/summary.json \
  --max-average 80.0
```

The program groups valid rows by `device_id` and calculates:

- `total_records`;
- `average_temperature` (rounded to two decimal places in the output);
- `error_count` for rows whose status is `ERROR` (case-insensitive).

Devices whose **unrounded** mean temperature is above `80.0` are excluded from the `devices` results. Invalid rows with a blank device ID or missing/non-numeric/non-finite temperature are skipped and counted. Missing input files and CSV files with missing required columns produce a clear error rather than an unhandled traceback.

The JSON contains a `devices` list, the configured temperature filter, IDs of filtered-out devices, and the number of skipped rows. The terminal output includes an aligned summary table.

## Section 3 - Generate the bar chart

After creating `output/summary.json`, run:

```bash
python -m src.chart \
  --input output/summary.json \
  --output output/average_temperature_by_device.png
```

The chart compares average temperature by device. It includes a title, axis labels, rotated/right-aligned device labels, a compact layout, and a message for an empty result set. The PNG is saved at the output path.

## Tests

Run all tests from the project directory:

```bash
python -m unittest discover -s tests -v
```

The tests cover normal and edge cases for sensor sanitization, temperature aggregation, case-insensitive error counting, filtering at the `80.0` threshold, malformed temperature rows, and missing required columns.

## Project structure

```text
.
├── device_telemetry.csv
├── output/
│   ├── summary.json
│   ├── sample_terminal_output.txt
│   └── average_temperature_by_device.png
├── src/
│   ├── sanitizer.py
│   ├── summarize.py
│   └── chart.py
├── tests/
│   ├── test_sanitizer.py
│   └── test_summarize.py
├── PROMPT_LOG.md
├── requirements.txt
└── README.md
```
