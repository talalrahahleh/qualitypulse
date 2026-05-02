# QualityPulse — Fahrzeugqualitäts-Dashboard

**Vehicle Quality Intelligence Dashboard** — A full-stack web application simulating a real-time automotive defect management and analytics system, modelled on OEM quality digitalization workflows.

> Built entirely in Vanilla JavaScript, HTML5, and CSS3 — no frameworks, no backend, no dependencies except Chart.js.

---

## Live Demo

Open `index.html` directly in any modern browser — no server required.

---

## Features

### Dashboard & KPIs
- Animated KPI counters: total defects, critical issues, resolution rate, average resolution time
- Data spans **60 simulated vehicle defect records** across **7 BMW model lines**

### Defect Tracker
- Multi-criteria filtering by severity, model, status, and free-text search
- Client-side sorting on all columns
- Pagination — no third-party table library used

### Analytics
Five interactive Chart.js visualizations:
- 12-week defect volume trend
- Per-model resolution rate comparison
- Monthly volume distribution
- Component category breakdown
- Severity-based resolution time analysis

### Custom SQL Query Engine
The standout feature: a SQL engine built from scratch in JavaScript supporting a meaningful subset of Oracle SQL syntax:

```sql
SELECT model, COUNT(*) AS total FROM defects WHERE severity = 'Critical' GROUP BY model ORDER BY total DESC LIMIT 5
```

**Supported syntax:**
- `SELECT` with column names and aliases
- `FROM` (in-memory defect dataset)
- `WHERE` with `=`, `!=`, `LIKE`, `AND`, `OR`
- `GROUP BY` with `COUNT(*)` and `AVG()` aggregate functions
- `ORDER BY` with `ASC` / `DESC`
- `LIMIT`

Results are rendered in a formatted table with execution time displayed in milliseconds.

---

## Tech Stack

| Layer | Technology |
|---|---|
| Frontend | JavaScript (ES6+), HTML5, CSS3 |
| Charts | Chart.js 4.4.1 |
| Database | Oracle SQL (custom in-memory engine) |
| UI | Dark theme, responsive grid layout, CSS animations |
| Fonts | Inter, Barlow Condensed, IBM Plex Mono |

---

## Data Model

Defect records contain the following fields:

| Field | Type | Description |
|---|---|---|
| `id` | String | Unique defect identifier (e.g. DEF-001) |
| `model` | String | BMW model line |
| `component` | String | Component category (ADAS, Electronics, Powertrain, etc.) |
| `severity` | String | Critical / High / Medium / Low |
| `status` | String | Open / In Progress / Resolved |
| `reported_date` | String | Date reported |
| `resolution_days` | Number | Days to resolve (null if open) |
| `description` | String | Defect description |

---

## Example SQL Queries

```sql
-- All critical defects
SELECT * FROM defects WHERE severity = 'Critical'

-- Average resolution time by model
SELECT model, AVG(resolution_days) AS avg_days FROM defects GROUP BY model ORDER BY avg_days ASC

-- Open defects in ADAS or Powertrain
SELECT id, model, component, severity FROM defects WHERE status = 'Open' AND (component = 'ADAS' OR component = 'Powertrain') LIMIT 10

-- Resolution rate by severity
SELECT severity, COUNT(*) AS total FROM defects GROUP BY severity ORDER BY total DESC
```

---

## Project Structure

```
qualitypulse/
├── index.html       # Complete application (single file)
└── README.md        # This file
```

---

## Motivation

This project was built to explore how automotive OEM quality management workflows can be digitalized through modern web technologies. The SQL engine was implemented from scratch as a learning exercise in query parsing and in-memory data processing.

---

## Author

**Talal Rahahleh**  
B.Sc. Computer Engineering — German Jordanian University (GJU)  
[linkedin.com/in/talal-rahahleh](https://www.linkedin.com/in/talal-rahahleh)
