# Lab 1 — Project Foundation & File Analysis

## Aim
To set up a Git-based cryptographic toolkit with a menu-driven CLI, file analysis module, and activity logging system.

## Brief Theory
A cryptanalysis toolkit requires modular architecture separating the CLI, processing logic, and data persistence. File analysis computes character/word/line counts and letter frequency (a–z) — the foundation for attacking substitution ciphers. Activity logging with timestamps ensures traceability of all operations.

## Algorithm/Flowchart

```mermaid
flowchart TD
    A([Start]) --> B[Initialize Git Repo & Folder Structure]
    B --> C[Display Menu: Encrypt / Decrypt / Attack / Analyze / Exit]
    C --> D[Capture User Selection]
    D --> E[Log Selection with Timestamp to activity.log]
    E --> F{User Choice?}
    F -->|Analyze| G[Prompt for File from datasets/]
    G --> H[Read File Contents]
    H --> I[Compute Char, Word, Line Count & Unique Chars]
    I --> J[Calculate Letter Frequency a-z]
    J --> K[Print Summary Report]
    K --> C
    F -->|Encrypt / Decrypt / Attack| L[Execute Selected Operation]
    L --> C
    F -->|Exit| M([Stop])
```

**Steps:**
1. Initialize Git repository and create folder hierarchy (`datasets/`, `logs/`, source modules).
2. Display interactive menu: Encrypt, Decrypt, Attack, Analyze, Exit.
3. Capture user selection and log with timestamp to `logs/activity.log`.
4. If Analyze → prompt for file from `datasets/`, read contents, compute stats (chars, words, lines, unique chars), calculate letter frequency (a–z), print report.
5. Loop back to menu until Exit.

## Important Commands
```bash
git init
git clone <repository_url>
pip install -r requirements.txt
python main.py
git add .
git commit -m "Initial commit: project foundation and file analysis"
git push origin main
```

## Observations

- Menu options Encrypt, Decrypt, Attack display "Coming Soon" as no cipher is implemented yet.
- File analysis correctly computes letter frequency distribution across all test datasets.
- Every menu selection is logged with an accurate timestamp in `logs/activity.log`.
