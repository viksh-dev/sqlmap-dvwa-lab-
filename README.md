# SQLMap Usage for SQL Injection — Lab Project

> **Disclaimer:** This project is carried out exclusively in a local isolated laboratory (DVWA in Docker) for educational purposes. No real websites or third-party systems were attacked. All techniques were applied only to a deliberately vulnerable educational application deployed on its own computer.

## About the project

A practical demonstration of the detection and exploitation of SQL injections using **sqlmap** using the example of a vulnerable web application **DVWA (Damn Vulnerable Web Application)**. The project shows a full cycle: from manual exploration to automated exploitation, including WAF bypass, and ends with a section on vulnerability mitigation.

## Project objectives

- Show an understanding of the SQL injection mechanism (error-based, blind, time-based)
- Demonstrate practical knowledge of sqlmap: from basic scans to fine-tuning (`--level`, `--risk`, `--tamper')
- Show the ability to work with real WAF (ModSecurity) and bypass its protection
- Give recommendations on how to fix vulnerabilities — not only "how to break", but also "how to fix"

## Stack

| Component | Purpose |
|-----------------|--------------------------------------|
| Docker / Docker Compose | Orchestration of all lab services |
| DVWA | Vulnerable web Application (target)       |
| MySQL | DVWA database |
| ModSecurity + nginx | WAF before DVWA (for the crawl stage) |
| sqlmap | Injection Automation tool |

## Repository structure

```
sqlmap-dvwa-lab/
├── README.md
├── docker-compose.yml
├── docs/
│   ├── 01-lab-setup.md # installing and checking the lab
,── 02-recon.md # manual exploration of injection points
,── 03-sqlmap-basics.md # basic scans (GET/POST, DATABASE dump)
│   ├── 04-injection-types.md  # error-based / blind / time-based
,── 05-waf-bypass.md # bypassing ModSecurity, --tamper scripts
,── 06-mitigation.md # recommendations for fixing vulnerabilities
├── screenshots/                # screenshots for each stage
├── waf/
│ └── modsecurity.conf # WAF configuration
└── .gitignore
```

## How to reproduce

1. Install Docker Desktop (Windows/macOS) or Docker Engine (Linux)
2. Clone the repository: `git clone <link>`
3. Launch the lab: `docker compose up -d`
4. Follow the steps in `docs/01-lab-setup.md` → `docs/06-mitigation.md` in order

## Content (by stages)

1. [Laboratory Preparation](docs/01-lab-setup.md)
2. [Manual Exploration](docs/02-recon.md)
3. [sqlmap Basics](docs/03-sqlmap-basics.md)
4. [Types of injections](docs/04-injection-types.md)
5. [WAF Bypass](docs/05-waf-bypass.md)
6. [Vulnerability Management](docs/06-mitigation.md)

The project was created for educational purposes. Use only in a controlled environment.
