# EV_Charging-Warsaw
Tableau EV Charging benchmarking and expansion analysis

Link: https://public.tableau.com/views/EVs_17609524518250/Expansion?:language=en-US&:sid=&:redirect=auth&:display_count=n&:origin=viz_share_link

Tools: Tableau, CSV/GeoJSON

Scope: 1-day exploratory analysis 

Dataset: Synthetic Warsaw EV charging data (Kaggle) - session records, station metadata, district geometry

## 1) Objective

Assess Warsaw’s EV charging activity from the perspective of a major network operator (“Greenway”) to:
 - benchmark Greenway vs competitors
 - identify high-use stations and districts
 - identify candidate areas for further investigation and potential expansion.

## 2) Key questions

 - What is the distribution of stations, and which show the highest usage? (kWh / plug / day)?
 - How does Greenway compare to other operators on utilisation and coverage?
 - Which locations merit follow-up (e.g. plug expansion, new stations)?

## 3) Data & core metrics

### Sources
 - charging_sessions.csv — session start time, kWh charged, cost per session
 - charging_stations.csv — station id, lat/long, district, operator, plug count
 - districts.geojson — Warsaw district boundaries
 - customers.csv — customer / vehicle context

### Key metric
 - Median kWh per plug per day (station level) - main proxy for station demand
 - Normalised by days in operation and plug count so stations of different ages are comparable.

### Time-banding

Pricing in the dataset aligns to session start time bands:

Off-peak: 20:00–07:59

Mid: 08:00–15:59

Peak price window: 16:00–19:59

Observed demand peak (session starts): 08:00–16:00 - commuter/daytime driven demand.

Important caveat: session duration and plug occupancy are not available; kWh/plug/day and session-starts/day are proxies for congestion and demand, not direct measures of occupied plug-hours.

## 4) Dashboards 

Dashboard 1 — Benchmarking (overview)
 - District density map: station counts per district, with Greenway stations overlaid.
 - KPIs / BANs: total sessions, total kWh, avg cost per kWh, mean sessions per plug, mean kWh per plug.
 - Operator bars: normalised metrics (kWh/station/day, sessions/station/day and average price/kwh) with Greenway highlighted.
 - Heatmap: session starts by day of week × hour to show hourly patterns and the daytime peak.

Dashboard 2 — Expansion & detail
 - District map (choropleth) plus stations coloured by median kWh/plug/day (adjustable color scale to focus on top stations).
 - Top N bars: stations ranked by median kWh/plug/day (Greenway highlighted).
 - Line chart: kWh/plug over time (top-N stations) with whole dataset upper-quartile reference line.
 - Peak & mid sessions per day: absolute, days-normalised counts of sessions starting in Mid (08–15) and Peak (16–19) windows - used to prioritise stations that attract daytime traffic and are most likely to have excess demand. Filtered for Top-N stations by mean mid sessions/day. 

## 5) Key findings
 - Daytime demand dominates: Session-start activity concentrates between 08:00–16:00, consistent with commuter patterns.
   Prices follow fixed time bands: Pricing is set by start-time buckets (off/mid/peak) rather than dynamically per station; price variation therefore largely reflects time-of-day mix rather than station-level pricing strategy.
 - Greenway is competitive: Greenway’s per-station performance is similar to peers overall. Differences between operators are modest.
 - Spatial opportunity: Many high-use stations are clustered in dense, central districts; however there are also high-use sites in less dense districts (e.g., Rembertów, Żoliborz) that may indicate other commuter hubs. Greenway has no station in one of these areas, Rembertów.
 - Timing vs intensity mismatch: 6 of the top 10 stations by median kWh/plug/day are not in the top 10 by mid-hour session starts. It’s possible this indicates some high-use stations accumulate energy outside of the main mid window, or have different session profiles. Duration/occupancy data is needed to determine which, and confirm true peak congestion. 

## 6) Interpretation & recommended next steps

This analysis provides candidate lists for further investigation. Recommended next actions:
 - Collect session duration / plug-occupancy logs to compute occupied plug-hours and true utilisation rates.
 - Estimate install & operating costs and model ROI for candidate stations/districts.
 - Pilot test capacity changes in 1–2 candidate districts (add plugs or a new station) and track occupancy and revenue impact.
 - Link external data (traffic flows, public transport hubs, population / workplace density) to refine location selection.

## 7) Limitations, assumptions:
 - No session end / duration or plug-level occupancy data - all temporal analyses are based on session start times and kWh charged per session.
 - Price is recorded at session start and appears fixed per start-time band - therefore we cannot isolate true within-session dynamic pricing effects (if they exist)

## 8) Notes:
 - Dataset is synthetic
 - All stations have the same plug count, hence per station metrics are comparable

## 9) Deliverables & files
- dashboards/ — Tableau workbook(s) (Benchmark + Expansion)
- data/ — CSVs and geojson used (cleaned, minimal)
- README.md — this document

