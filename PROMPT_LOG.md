# AI Prompt Log

This log records the prompts and review adjustments for the charting portion of the Device Log Summarizer assessment.

## 1. Initial prompt

> Write a Python script that loads `output/summary.json` from the Device Log Summarizer, reads the average temperature for each device, and creates a Matplotlib bar chart comparing average temperature by device. Include a clear chart title and axis labels, save the image as `output/average_temperature_by_device.png`, and handle a missing JSON file or an empty `devices` list with a readable error or message.

## 2. Issue found during review

The first chart draft used a small figure and horizontal device IDs without a layout adjustment. The x-axis title was clipped at the bottom of the saved PNG, and the long tick labels were crowded. This showed that the saved figure needed more room and an explicit layout adjustment.

## 3. Follow-up prompt

> Fix the chart layout so longer device IDs do not overlap: rotate x-axis labels about 35 degrees, right-align them, use a figure size suitable for the labels, and call `fig.tight_layout()` before saving. Keep the title and axis labels, and display an informative message when no devices pass the temperature filter.

## 4. Final review changes

- Rotated and right-aligned x-axis tick labels.
- Added `tight_layout()` and saved with a tight bounding box to reduce clipping.
- Added an empty-results message and JSON-structure validation.
- Ran the script against the sample JSON and checked that the PNG was created.
- Ran the automated tests for the sanitization and aggregation functions.
