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

```mermaid
flowchart TD
    A([Start]) --> B[Display Login Prompt]
    B --> C[Enter Username & Password]
    C --> D{Credentials Valid?}
    D -->|No| E[Leak Valid Credentials in Debug Output]
    E --> B
    D -->|Yes| F[Show CRUD Menu: Add / View / Search / Delete / Exit]
    F --> G{User Choice?}
    G -->|Add| H[Store Entry in Plaintext File]
    H --> F
    G -->|View / Search| I[Read & Display from Plaintext File]
    I --> F
    G -->|Delete| J[Remove Entry from File]
    J --> F
    G -->|Exit| K[Run Bandit on Source Code]
    K --> L[Compare Findings vs Embedded Vulnerabilities]
    L --> M([Stop])
```

**Steps:**
1. Launch Password Manager CLI → display login prompt.
2. Verify credentials against hardcoded admin username/password.
3. On failure → leak valid credentials in debug output (intentional vulnerability).
4. On success → present CRUD menu: Add, View, Search, Delete, Exit.
5. All entries stored/retrieved in plaintext (intentional vulnerability).
6. Run Bandit on source → compare detected vs embedded vulnerabilities to identify SAST blind spots.

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
