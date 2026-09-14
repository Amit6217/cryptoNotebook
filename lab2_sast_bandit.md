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
1. Install Bandit in the environment using `pip`.
2. Author a vulnerable Python test script embedding 5 distinct flaws (`B101`, `B105`, `B303`, `B311`, `B602`).
3. Initiate session recording using `script sast_lab_log.txt`.
4. Execute Bandit scanner against the target test script in recursive mode.
5. Generate formal scan reports in plain text (`.txt`) and structured JSON (`.json`) formats.
6. Exit the `script` recording session and display the recorded log file.
7. Review identified issues, severity levels, and suggested remediations.

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
