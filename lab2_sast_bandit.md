# Lab 2 — Static Application Security Testing (SAST) using Bandit

## Aim
To install and configure the Bandit SAST tool, construct a deliberately vulnerable Python program with common security flaws, scan source code to identify vulnerabilities, and capture terminal audit logs.

## Brief Theory
Static Application Security Testing (SAST) analyzes source code without execution to identify security flaws early in development.
Bandit is a dedicated security linter for Python that builds an Abstract Syntax Tree (AST) to evaluate code against known vulnerability plugins.
Key vulnerabilities detected include hardcoded credentials (`B105`), reliance on `assert` statements for authorization (`B101`), and obsolete hashing functions like MD5 (`B303`).
It flags command injection risks from executing subprocesses with `shell=True` (`B602`) and pseudo-random number generators used in security contexts (`B311`).
SAST facilitates automated security auditing, reporting vulnerabilities categorized by severity (Low, Medium, High) and confidence levels.

## Algorithm/Flowchart

```mermaid
flowchart TD
    A([Start]) --> B[Install Bandit via pip]
    B --> C[Write Vulnerable Test Script with 5 Flaws]
    C --> D[Start Terminal Recording: script sast_lab_log.txt]
    D --> E[Run Bandit Scanner on Test Script]
    E --> F[Generate Reports: TXT + JSON]
    F --> G[Exit script Session]
    G --> H[Review Findings: Severity & Remediation]
    H --> I([Stop])
```

**Steps:**
1. Install Bandit using `pip install bandit`.
2. Write a vulnerable Python script embedding 5 flaws (B101, B105, B303, B311, B602).
3. Start terminal recording with `script sast_lab_log.txt`.
4. Run Bandit scanner in recursive mode on the test script.
5. Export reports in TXT and JSON formats.
6. Exit recording session and review findings (severity, remediation).

## Important Commands
```bash
pip install bandit
bandit -r tests/vulnerable_test.py
bandit -r file.py -f txt -o report.txt
bandit -r file.py -f json -o report.json
script sast_lab_log.txt
exit
cat sast_lab_log.txt
```

## Observations
- Bandit successfully scanned the Python codebase and generated both text and JSON audit reports.
- Detected 7 findings categorized across Low, Medium, and High severity ratings.
- All 5 intentionally embedded security vulnerabilities were accurately identified and mapped to Bandit test IDs.
