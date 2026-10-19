# Server Anomaly Detection (AI-Assisted Analysis)

Statistical anomaly detection on server monitoring data, built using an 
AI-assisted Python workflow in Google Colab

## Overview
Analyzed 16,000 monitoring readings (15-minute intervals, ~3 weeks) across 8 
production servers — tracking CPU usage, memory usage, response time, and 
error counts. Used an AI coding assistant (ChatGPT) to generate and refine a 
Python script that statistically flags performance anomalies, then reviewed 
and validated the results.

## AI-Assisted Workflow
This project intentionally demonstrates directing an AI tool to do real 
analytical work, not just asking general questions. The process:

1. **Prompted the AI** to write a Python/pandas script that calculates each 
   server's baseline mean and standard deviation for CPU, memory, and response 
   time, then flags any reading more than 3 standard deviations above that 
   server's own baseline as an anomaly (rather than a single fixed threshold 
   across all servers, which would be inaccurate — different servers have 
   different normal operating loads).
2. **Ran the generated script** in Google Colab against the raw dataset and 
   reviewed the output for correctness (anomaly counts, per-server breakdown).
3. **Prompted a follow-up** to visualize the results: a bar chart of anomaly 
   counts per server, and a time-series chart highlighting flagged anomalies 
   on the server with the highest incident count.
4. **Verified the results** made operational sense: ~2.6% of readings flagged 
   (412 of 16,000), consistent with realistic incident rates for a monitored 
   server fleet, with anomalies properly distributed across all 8 servers 
   rather than concentrated in a way that would suggest a bug in the logic.

## Key Findings
- **412 anomalies detected** out of 16,000 readings (~2.6%), using a 
  per-server statistical baseline (3-sigma threshold).
- **DB-PROD-02 had the most anomalies (76)**, followed by FILE-PROD-01 (61) 
  and APP-PROD-02 (55) — a reasonable spread suggesting no single server is 
  catastrophically unstable, but database and file servers trend higher.
- Anomalies were distributed across all 8 servers (40–76 each), confirming 
  the detection logic is server-aware rather than biased toward high-traffic 
  servers.

## Tools Used
Python (pandas, matplotlib), Google Colab, ChatGPT (AI-assisted script 
generation and iteration)

## Files
- `server_monitoring_data.csv` — raw monitoring dataset (16,000 readings)
- `server-anomaly-detection.ipynb` — analysis notebook
- `detected_anomalies.csv` — flagged anomaly rows
- `anomalies_by_server.png` — anomaly count by server (chart)
- `response_time_anomalies.png` — response time over time with anomalies highlighted (chart)

## Data Source
Synthetic server monitoring dataset modeled on real-world 24/7 IT monitoring 
operations
