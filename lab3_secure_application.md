# Lab 3 — Secure Application Development (Password Manager)

## Aim
To develop a console-based Password Manager in Python with authentication and credential management features, embed intentional security vulnerabilities, and evaluate the detection capabilities and limitations of Bandit static analysis.

## Brief Theory
Secure application development demands robust authentication, defense against data leakage, and secure persistent storage.
Password managers handle high-value secrets, requiring encrypted storage at rest and strict sanitization of diagnostic output.
Static code analysis tools like Bandit inspect syntactic structures (AST nodes) for known signature patterns such as hardcoded credentials (`B105`).
However, static tools cannot comprehend execution context, semantic intent, or higher-level business logic.
Consequently, flaws such as plaintext file persistence and verbose error leaks bypass automated SAST scanners, underscoring the necessity of complementary manual code reviews.

## Algorithm/Flowchart
1. Launch the Password Manager CLI and display login prompt.
2. Verify user credentials against hardcoded admin username and password.
3. If login fails, emit verbose debug message leaking the correct admin credentials.
4. If login succeeds, present CRUD operations: Add, View, Search, Delete entries, or Exit.
5. Save and retrieve password entries directly in plaintext format on persistent storage.
6. Execute Bandit SAST scanner against the application source code.
7. Compare detected findings with embedded vulnerabilities to identify SAST blind spots.

## Important Commands
```bash
python password_manager.py
bandit -r secure_application/src/password_manager.py
bandit -r file.py -f txt -o report.txt
```

## Observations
- The Password Manager functioned correctly across all credential management operations.
- Bandit detected only the hardcoded credentials (`B105`), catching 1 out of 3 embedded vulnerabilities.
- Logic-level flaws (debug information leakage and plaintext credential storage) were undetected by static analysis, proving that manual security review is essential.
