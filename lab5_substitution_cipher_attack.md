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
1. Read cleaned ciphertext input.
2. Execute `frequency_analysis()` to compute letter counts and percentages, sorted in descending order.
3. Execute `word_frequency_analysis()` to extract and rank single-letter, two-letter, three-letter, and repeated words.
4. Execute `pattern_analysis()` to detect letter pattern signatures (e.g., ABCCD) and doubled letters.
5. Provide interactive cryptanalysis menu:
   1. Propose substitution mapping ($C \to P$).
   2. Invoke `apply_substitution()` to replace mapped letters in ciphertext, filling unmapped characters with `'?'`.
   3. Call `display_partial_plaintext()` to display side-by-side ciphertext and partial plaintext with the substitution table.
   4. Undo incorrect guesses or auto-propose initial high-probability mappings as needed.
6. Iteratively refine substitutions using English grammatical and contextual clues until all 26 letters are resolved.
7. Call `verify_solution()` to re-encrypt recovered plaintext with the final key and verify an exact match with original ciphertext.

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
