# ST5041CMD [Course]
> The Internet and Web Technologies.

---

## [~] Web Honeypot with Attack Dashboard

A fake system that looks real and valuable, but exists only to be attacked.

| Course | ST5041CMD - The Internet and Web Technologies |
|---|---|
| Lecturer | Abhishek Bimali |
| Student | Aditya Shrestha [250498] |
| Project Type | Final project (defensive + offensive, purple team) |
| Purpose | Capture, classify and visualise web attacks against an isolated decoy site. |

---

## [~] Overview

A `honeypot` is a decoy that no real user has a reason to visit, a every request to it is suspicious.

**This project has two parts:**

* Decoy website: a fake site with tempting targets (fake admin logs, fake `/.env`, fake search box). It records everything sent to it and never executes attacker input.
* Attack dashboard: a private, secured analyst page that turns that logs into charts, tables and reports.

**Goals:**

* Detect and classify common web attacks (SQLi, XSS, path traversal, scanning, brute force).
* Learn how real attacks look at the HTTP level.
* Measure detection quality (detection rate, false positives).
* Apply secure web development to the project itself.

---

## [~] How It Works

```
Attacker / bot
      ↓  HTTP request
Decoy web app (Flask)         → fake pages, fake login, catch-all route
      ↓
Logger + Classifier           → tags: sqli, xss, traversal, scanner, brute_force
      ↓
Enrichment                    → GeoIP country, tool guess from User-Agent
      ↓
Database (SQLite)             → events, ips, credentials_tried, rules
      ↓
Dashboard (private)           → login + MFA, charts, live feed, export
```

---

## [~] Repository Structure

```
ST5041CMD-Honeypot/
│── decoy/                  → Public fake site (isolated)
│ │── app.py                → Flask app + catch-all route
│ │── logger.py             → Captures each request
│ │── classifier.py         → Attack detection rules
│ │── enrich.py             → GeoIP + User-Agent parsing
│ └── fake_responses/       → Fake .env, fake admin pages
│
│── dashboard/              → Private analyst site
│ │── app.py                → Login, MFA, routes
│ │── templates/            → Jinja2 pages (auto-escaped)
│ │── static/               → Chart.js, CSS
│ └── reports.py            → PDF / CSV export
│
│── shared/
│ │── db.py                 → Database helpers (parameterized queries only)
│ └── config.py             → Settings from environment variables
│
│── tests/
│ │── test_classifier.py
│ │── attack_payloads.txt   → Labelled attack + benign inputs
│ └── run_attacks.py        → Fires payloads, calculates metrics
│
│── data/                   → Database + GeoIP file (not committed, see data/README.md)
│── docs/                   → Diagrams, threat model, report, screenshots
│── docker-compose.yml
│── requirements.txt
└── README.md               → Project overview
```

Each major folder contains its own notes where needed. See [`data/README.md`](data/README.md) for what is stored there.

---

## [~] Features

**Decoy:**

* Rule-based classification: SQLi, XSS, path traversal, scanner, brute force.
* URL decoding before matching (handles encoded payloads).
* Severity scoring and repeat-offender tracking.

**Dashboard:**

* Login with password hashing and TOTP MFA.
* Attacks over time, top attack types, top IPs/countries, targeted paths, credentials tried.
* Live feed, filters, attack journey view.
* PDF / CSV export.

---

## [~] Tech Stack

| Area | Tools |
|---|---|
| Language | Python 3.10+, SQL, HTML/CSS, JavaScript |
| Web | Flask, Jinja2 |
| Database | SQLite (PostgreSQL optional) |
| Enrichment | geoip2 (MaxMind GeoLite2), user-agents |
| Security | Flask-Login, Flask-WTF (CSRF), Flask-Limiter, pyotp, Werkzeug hashing |
| Charts | Chart.js (hosted locally) |
| Testing | pytest, requests, Burp Suite, sqlmap, Nikto, gobuster, hydra, OWASP ZAP |
| Deployment | Docker, docker-compose |

---

## [~] Getting Started


---