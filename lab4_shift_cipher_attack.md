# Lab 4 — Cryptanalysis of Shift (Caesar) Cipher

## Aim
To implement cryptanalysis of the Shift Cipher using Chi-Square analysis and Brute Force with Dictionary Scoring.

## Brief Theory
The Shift Cipher encrypts via $E(x) = (x + k) \bmod 26$ with key space of only 26 values. Chi-square analysis compares candidate decryption frequencies against English frequencies: $\chi^2 = \sum \frac{(O_i - E_i)^2}{E_i}$; the key with minimum χ² is selected. Dictionary scoring decrypts with all 26 keys and selects the key whose plaintext has the most English dictionary word matches.

## Algorithm/Flowchart

```mermaid
flowchart TD
    A([Input Ciphertext C]) --> B[/"For each key k = 0 to 25"/]
    B --> C["Decrypt: P_k = D(C, k)"]
    C --> D1["Chi-Square Attack"]
    C --> D2["Dictionary Attack"]

    D1 --> E1["Count letter frequencies O_i in P_k"]
    E1 --> F1["Compute expected E_i = N x f_English"]
    F1 --> G1["Calculate chi-square score"]
    G1 --> H1["Select k with lowest chi-square"]

    D2 --> E2["Split P_k into words"]
    E2 --> F2["Count matches in english_words.txt"]
    F2 --> G2["Select k with highest word-match count"]

    H1 --> I([Output Key & Plaintext])
    G2 --> I
```

**Steps:**
1. Input ciphertext C.
2. **Chi-Square Attack:** For each k ∈ [0, 25] → decrypt → count letter frequencies → compute χ² against English frequencies → select k with lowest χ².
3. **Dictionary Attack:** For each k ∈ [0, 25] → decrypt → split into words → count dictionary matches → select k with highest count.
4. Output predicted keys and decrypted plaintexts.

## Important Commands
```bash
# Navigate to attack directory
cd attacks/shift_cipher_attack

# Run single test or batch mode
python main.py
```

## Observations

### Results Table

| TC# | Plaintext | Actual Key | χ² Key | χ² Score | χ² Correct? | Dict Key | Dict Score | Dict Correct? |
|:---:|-----------|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| 1 | HELLO WORLD | 3 | 6 | 24.01 | ❌ | 3 | 2 | ✅ |
| 2 | ATTACK AT DAWN | 5 | 5 | 33.12 | ✅ | 5 | 1 | ✅ |
| 3 | CRYPTOGRAPHY IS FUN | 10 | 23 | 23.99 | ❌ | 10 | 2 | ✅ |
| 4 | THIS IS A SIMPLE TEST | 7 | 7 | 21.82 | ✅ | 7 | 3 | ✅ |
| 5 | THE QUICK BROWN FOX... | 12 | 12 | 109.32 | ✅ | 12 | 2 | ✅ |
| 6 | MEET ME AT THE STATION | 4 | 4 | 20.44 | ✅ | 4 | 1 | ✅ |
| 7 | COMPUTER SECURITY IS IMPORTANT | 8 | 8 | 22.79 | ✅ | 8 | 3 | ✅ |
| 8 | INFORMATION SECURITY | 15 | 15 | 12.47 | ✅ | 15 | 1 | ✅ |
| 9 | SHIFT CIPHER IS EASY... | 6 | 6 | 9.62 | ✅ | 6 | 3 | ✅ |
| 10 | THIS IS A CRYPTOGRAPHY LAB | 20 | 20 | 23.83 | ✅ | 20 | 3 | ✅ |

### Comparison

| Method | Accuracy | Strength | Weakness |
|--------|:--------:|----------|----------|
| Chi-Square | 8/10 (80%) | Fast, no dictionary needed | Fails on short texts (<20 chars) |
| Dictionary | 10/10 (100%) | Robust even on short texts | Needs a word list; slower |

### Failure Analysis
- **TC1** (10 letters) and **TC3** (16 letters) failed under χ² because small sample sizes don't conform to standard English frequency distribution.
- **Improvement:** Combine both methods — use dictionary scoring as fallback when text length < 20 characters.

### Conclusion
The Shift Cipher's key space (26) makes it trivially breakable. Dictionary scoring is more reliable overall. Chi-square is effective for longer texts but unreliable for short ones. A hybrid approach combining both methods gives optimal results.
