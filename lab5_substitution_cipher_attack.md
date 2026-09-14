# Lab 5 — Monoalphabetic Substitution Cipher Cryptanalysis

## Aim
To implement a Monoalphabetic Substitution Cipher and perform its cryptanalysis using frequency analysis, word patterns, and an interactive key-recovery interface in C++.

## Brief Theory
A Monoalphabetic Substitution Cipher maps each plaintext letter to a fixed ciphertext letter using a permutation of the 26-letter alphabet.
Although the key space is large ($26! \approx 4.03 \times 10^{26}$), rendering brute-force search infeasible, letter distributions remain unchanged.
Ciphertext letters preserve the characteristic frequencies of the underlying natural language (e.g., 'E', 'T', 'A' in English).
Statistical cryptanalysis exploits single-letter frequency rankings, word structures (e.g., common 1/2/3-letter words like "A", "OF", "THE"), and repeated patterns (e.g., doubled letters, pattern forms like ABCCD).
By iteratively proposing letter substitutions, inspecting the partial plaintext, and correcting mappings, the secret key can be completely recovered without searching the full key space.

## Algorithm/Flowchart

```mermaid
flowchart TD
    A([Input Ciphertext]) --> B["frequency_analysis(): Letter counts & percentages"]
    B --> C["word_frequency_analysis(): 1/2/3-letter & repeated words"]
    C --> D["pattern_analysis(): Letter patterns & doubled letters"]
    D --> E{All 26 Letters Mapped?}
    E -->|No| F["Propose Substitution: C → P"]
    F --> G["apply_substitution(): Replace known, show ? for unknown"]
    G --> H["display_partial_plaintext(): Side-by-side view"]
    H --> I{Mapping Correct?}
    I -->|No| J[Undo Guess]
    J --> E
    I -->|Yes| E
    E -->|Yes| K["verify_solution(): Re-encrypt & compare"]
    K --> L([Output Recovered Key & Plaintext])
```

**Steps:**
1. Read ciphertext → run `frequency_analysis()`, `word_frequency_analysis()`, `pattern_analysis()`.
2. Interactive loop: propose substitution (C→P) → `apply_substitution()` → `display_partial_plaintext()`.
3. If wrong → undo guess. If correct → continue until all 26 letters mapped.
4. `verify_solution()`: re-encrypt recovered plaintext, confirm exact match with original ciphertext.

## Important Commands
```bash
# Compile cryptanalysis program
g++ -std=c++17 -Wall -Wextra -Wpedantic main.cpp cipher.cpp analysis.cpp utils.cpp substitution.cpp -o substitution_cipher

# Execute interactive attack
./substitution_cipher
```

## Observations
- Single-letter frequency analysis accurately provides initial hypotheses for high-frequency vowels and consonants (e.g., cipher letter corresponding to 'E' and 'T').
- Short word frequency and pattern analysis serve as high-confidence anchors (e.g., identifying "THE", "AND", "THAT"), resolving multiple letters simultaneously.
- Interactive substitution with undo capability allows systematic key convergence even for low-frequency characters.
- Full plaintext was successfully recovered, and re-encryption verification passed with 100% exact match.
