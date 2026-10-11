# ST5041CMD [Course]
> The Internet and Web Technologies.

---

## [~] Web Honeypot with Attack Dashboard

A fake system that looks real and valuable, but exists only to be attacked.

| Course | ST5041CMD - The Internet and Web Technologies |
|---|---|
| Lecturer | Abhishek Bimali |
| Purpose | Capture, classify and visualise web attacks against an decoy site. |

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
      ↓
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
ST5041CMD/
│── dashboard/                → Private analyst site: login + MFA, charts, live feed
│ │── static/                 → CSS and Javascript
│ │── templates/              → Jinja2 pages (auto-escaped)
│
│── data/                     → Runtime storage: database, raw logs, GeoIP file (non-commited)
│
│── decoy/                    → Publi fake site that logs and classifies every request
│ │── response/               → Fake files served to attackers (.env, config.php, robots.txt)
│ │── static/                 → CSS for the fake pages
│ │── templates/              → Fake pages (admin login, phpMyAdmin, search, 404)
│
│── document/                 → Project documentation and report evidence
│ │── demo/                   → Demo video link and notes
│ │── diagrams/               → Architecture, data flow, ER and sequence diagrams
│ │── draft/                  → Local drafts
│ │── report/                 → Final report
│ │── screenshot/             → Screenshots (masked IP addresses)
│
│── logs/                     → Runtime log files (non-commited)
│
│── scripts/                  → Helper scripts: admin, backup, reset lab, test traffic
│
│── shared/                   → Code used by both apps: config, database, security helpers
│
│── tests/                    → Automated tests and the attack / benign payload lists
│ │── results/                → Test output (summary files)
│
│── docker-compose.yml        → Starts decoy and dashboard together
│── LICENSE                   → Terms and Conditions
│── pytest.ini                → Pytest settings (test folder)
│── README.md                 → Project overview
│── requirements-dev.txt      → Extra packages for testing and development
│── requirements.txt          → Packages needed to run the project
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

## [~] Security of the Project Itself

* Parameterized SQL queries only.
* Output encoding on all logged data (stops store XSS)
* Password hashing, MFA, lockout, secure cookie flags.
* CSRF tokens on all dashboard POST requests.
* Rate limiting and size limits (log flooding protection).
* Decoy runs as non-root with outbound traffic blocked.
* Decoy and dashboard on separate networks; least-privilege database users.
* Secrets only in environment variables.

---

## [~] Ethics and Legal

* Attack only systems I own or have written permission to test.
* No real malware; web-shell tests use harmless dummy files.
* The honeypot is passive: no 'hacking back'.
* IP addresses may be personal data (UK GDPR): minimised, retention-limited, masked in reports.
* Relevant law: Computer Misuse Act 1990 (UK) or local equivalent.

---

## [?] Limitations

* Only sees attackers who find and touch the decoy.
* Signature-based rules miss obfuscated payloads.
* Most traffic is automated noise, not targeted attacks.
* Skilled attackers may fingerprint the fake.
* A local-only lab shows my own attacks, not real-world ones.
* SQLite has no per-user permissions, so decoy and dashboard share on database file.

---

## [?] Future Work

* MITRE ATT&CK heatmap of observed techniques.
* Alerts via email or Telegram.
* Hash-chained logs for tamper evidence.
* PostgresSQL with separate database roles.

---

## [~] How to Navigate

* Start here for the project overview.
* Open `decoy/` for the fake site, logger and classifier.
* Open `dashboard/` for the analyst interface.
* Open `tests/` for payload tests and metrics.
* Open `data/README.md` for storage, privacy and GeoIP setup.
* Oepn `docs/` for diagrams, the threat model and the report.

---

## [~] References

* OWASP Top 10 and OWASP Cheat Sheet Series
* MITRE ATT&CK
* Honeynet Project; Cowrie, T-Pot, OpenCanary (studied, not copied)