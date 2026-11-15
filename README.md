# AstroVital: Astronaut Health Monitoring Software

NASA Space Apps Challenge 2026 | Problem statement: *Create Health Monitoring Software for Astronauts on Space Missions*

AstroVital is an edge-first health monitoring system that fuses wearable physiology, activity and cabin-environment data into a personal readiness score, so crews and ground teams can spot health risks early, even when Earth is minutes away.

**Live demo:** [add your Vercel / Streamlit link]
**Demo video:** [add your video link]

## Why it matters

Long missions bring microgravity effects, disrupted sleep, heavy exercise demands, radiation and confined living. On deep-space missions, communication delays make real-time ground support impossible, so monitoring has to work on board with little bandwidth.

## What it does

1. **Activity recognition:** classifies sleep, work, rest, treadmill, cycling and resistive exercise from accelerometer, gyroscope and heart rate.
2. **Intensity and exercise load:** heart-rate-reserve zones, and minutes in zone per sleep cycle.
3. **Personal baseline and anomaly detection:** learns each crew member's normal resting heart rate by hour of day, then flags persistent deviations (EWMA z-score) and multivariate outliers (Isolation Forest).
4. **Readiness score (0-100):** sleep, heart-rate deviation, exercise load and cabin environment, with visible weights.
5. **Alerts:** GREEN / AMBER / RED, each with a plain-language reason.
6. **Dashboard:** crew view and ground-team view.

## Architecture

```
Sensing -> Edge processing -> Analytics engine -> Alerts and decision support -> Dashboard -> Ground sync
```

## Repository layout

```
src/
  astro_mission_pipeline.py   # simulated mission timeline, features, activity/intensity models, context features
  pamap2_pipeline.py          # same pipeline on real PAMAP2 data (leave-one-subject-out validation)
  astro_health_module.py      # baseline, anomaly detection, readiness score, alerts
data/
  health_timeline.json        # hourly output used by the dashboard
docs/
  framework.md                # full framework document
README.md
requirements.txt
```

## Quick start

```bash
pip install -r requirements.txt

# 1. Simulated 5-day mission
python src/astro_mission_pipeline.py --days 5 --out out/

# 2. Baseline, anomaly detection, readiness and alerts (with a synthetic test anomaly)
python src/astro_health_module.py --features out/window_features_context.csv --inject --out out/

# 3. Optional: real-data validation on PAMAP2
python src/pamap2_pipeline.py --data-dir "PAMAP2_Dataset/Protocol" --placement hand --remove-gravity --out out_pamap2/
```

`requirements.txt`: `numpy`, `pandas`, `scikit-learn`, `matplotlib`

## Data sources

| Source | Use | Status |
|---|---|---|
| PAMAP2 (Reiss & Stricker, DFKI; hosted by UCI) | Real accelerometer, gyroscope, heart rate and activity labels for model validation | Real |
| NASA ISS Live cabin telemetry | Cabin environment values | [state whether used] |
| NASA OSDR (osdr.nasa.gov) | Spaceflight health context | [state whether used] |
| ISS-style cabin environment and crew timeline | Simulated demo data | **Simulated** |

**Citation:** A. Reiss and D. Stricker, "Introducing a New Benchmarked Dataset for Activity Monitoring," ISWC 2012.

## Results

| Test | Result |
|---|---|
| Activity recognition, leave-one-subject-out on PAMAP2 | [fill in after running `pamap2_pipeline.py`] |
| Anomaly detection on a simulated injected heart-rate anomaly (6 h, +12 bpm) | 5 of 6 hours flagged, no false alarms, first alert 1 h after onset |

The anomaly result comes from simulated data and is not real-world accuracy.

## Limitations

- The crew timeline and the cabin environment layer are simulated, unless replaced by the NASA sources above.
- PAMAP2 is Earth-based data from healthy young adults; spaceflight validation would need NASA Life Sciences Data Archive or analog-mission data.
- Readiness weights and alert thresholds are starting values, not clinically validated.
- AstroVital is decision support, not a medical device, and it does not diagnose.

## Roadmap

- Validate on analog-mission data (for example HERA, SIRIUS, HI-SEAS)
- Add sleep staging and radiation-exposure tracking
- Run on-board on edge hardware, with priority-based ground sync

## Team

[Names and roles]

## AI tools used

[Describe the AI tools used and for what, as the Space Apps rules require.]

## License

[Choose a license, for example MIT]
