# QualityPulse — Fahrzeugqualitäts-Dashboard

**Vehicle Quality Intelligence Dashboard** — A single-page, entirely client-side web application simulating an automotive defect management and analytics workflow, modelled on OEM quality digitalization processes.

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
The standout feature: a hand-written SQL-subset query interpreter, built from scratch in JavaScript with no libraries. It parses each clause with regular expressions and evaluates the result against an in-memory dataset. The syntax is ANSI-style and not Oracle-specific:

```sql
SELECT model, COUNT(*) AS total FROM defects WHERE severity = 'Critical' GROUP BY model
```

**Supported syntax:**
- `SELECT` with column names and aliases
- `FROM` (in-memory defect dataset)
- `WHERE` with `=`, `!=`, `LIKE`, `AND`, `OR`
- `GROUP BY` with `COUNT(*)` and `AVG()` aggregate functions
- `ORDER BY` with `ASC` / `DESC`
- `LIMIT`

Results are rendered in a formatted table with execution time displayed in milliseconds.

**Known limitations** (it is a regex interpreter, not a real parser):
- `ORDER BY` and `LIMIT` are ignored when `GROUP BY` is present — grouped results come back unsorted and unlimited.
- `WHERE` has no operator precedence and does not support parentheses; `AND` / `OR` are folded strictly left to right.
- Aliases (`AS`) are only applied inside `GROUP BY` projections.
- A condition the regexes cannot parse evaluates to `true` rather than raising an error.

---

## Tech Stack

| Layer | Technology |
|---|---|
| Frontend | JavaScript (ES6+), HTML5, CSS3 |
| Charts | Chart.js 4.4.1 |
| Data layer | In-memory JavaScript array (60 generated records) |
| UI | Dark theme, responsive grid layout, CSS animations |
| Fonts | Inter, Barlow Condensed, IBM Plex Mono |

---

## Data Model

Defect records contain the following fields:

| Field | Type | Description |
|---|---|---|
| `id` | String | Unique defect identifier (e.g. DEF-001) |
| `description` | String | Defect description |
| `model` | String | BMW model line |
| `component` | String | Component category (ADAS / Safety, Electronics, Powertrain, etc.) |
| `severity` | String | Critical / High / Medium / Low |
| `status` | String | Open / In Progress / Resolved / Closed |
| `assignee` | String | Engineer the defect is assigned to |
| `created` | String | Date reported, `YYYY-MM-DD` |
| `resolution_hrs` | Number | Hours to resolve (`null` while Open or In Progress) |

---

## Example SQL Queries

```sql
-- All critical defects
SELECT * FROM defects WHERE severity = 'Critical'

-- Average resolution time by model
SELECT model, AVG(resolution_hrs) AS avg_hrs FROM defects GROUP BY model

-- Recent open defects (see Known Limitations re: parentheses)
SELECT id, model, component, severity, assignee FROM defects WHERE status = 'Open' ORDER BY created DESC LIMIT 10

-- Defect count by severity
SELECT severity, COUNT(*) AS total FROM defects GROUP BY severity
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
