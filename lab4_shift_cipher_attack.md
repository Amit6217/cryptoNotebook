# Lab 4 — Cryptanalysis of Shift (Caesar) Cipher

## Aim
To implement and evaluate two cryptanalysis methods (Chi-Square Statistical Analysis and Brute Force with Dictionary Scoring) on the Shift Cipher in Python.

## Brief Theory
The Shift (Caesar) Cipher encrypts letters via $E(x) = (x + k) \pmod{26}$ and decrypts via $D(y) = (y - k) \pmod{26}$ for key $k \in [0, 25]$.
Because the key space is restricted to only 26 possible values, exhaustive search is trivial.
Chi-square analysis measures the statistical discrepancy between candidate decryption letter frequencies and standard English frequencies:
$$\chi^2 = \sum_{i=0}^{25} \frac{(O_i - E_i)^2}{E_i}$$
The key yielding the minimum $\chi^2$ value corresponds to the most statistically probable English plaintext.
Brute-force with dictionary scoring decrypts the ciphertext under all 26 keys and counts matching valid words against an English word list.
The candidate key maximizing the recognized word count is identified as the correct encryption key.

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
- Both attack methods correctly recover the shift key for typical English ciphertexts.
- Chi-square statistical attack is computationally fast and reliable for texts with $\ge 20$ letters, but can misidentify keys on short texts due to statistical noise.
- Dictionary scoring works robustly even on short ciphertexts provided the plaintext contains recognizable words in the dictionary.
