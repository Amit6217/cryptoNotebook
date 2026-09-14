# Lab 6 — Cryptanalysis of Vigenère Cipher

## Aim
To implement cryptanalysis of the polyalphabetic Vigenère Cipher using Kasiski Examination, Index of Coincidence (IC), and Chi-Square frequency analysis in C++.

## Brief Theory
The Vigenère Cipher shifts plaintext letters cyclically by key $K$ of length $m$: $C_i = (P_i + K_{i \pmod m}) \pmod{26}$.
Repeated plaintext segments separated by distances that are multiples of key length $m$ produce identical ciphertext segments.
Kasiski Examination finds repeated 3–5 character substrings, calculates spacing distances, and factors them to tally votes for probable key length $m$.
The Index of Coincidence (IC) calculates the probability that two randomly selected letters are identical:
$$IC = \frac{\sum_{i=0}^{25} f_i(f_i - 1)}{N(N - 1)}$$
English text has $IC \approx 0.0667$ while random text has $IC \approx 0.0385$. Group IC averages confirm the correct key length ($m = 14$, $IC \approx 0.0644$).
Partitioning ciphertext into $m$ independent cosets reduces the polyalphabetic cipher into $m$ monoalphabetic Caesar ciphers, solvable via chi-square goodness-of-fit against standard English frequencies.

## Algorithm/Flowchart
1. Preprocess ciphertext with `clean_ciphertext()` to retain only uppercase letters A–Z.
2. Determine key length using Kasiski Examination:
   1. Detect repeated 3–5 character sequences using `find_repeated_patterns()`.
   2. Calculate intervals between matching sequences with `calculate_distances()`.
   3. Find integer factors of distances using `find_factors()`.
   4. Tally factor votes across candidate key lengths (2–20) via `kasiski_analysis()`.
3. Validate candidate key length using `calculate_ic()` to select length $m$ with average IC closest to English ($m = 14$, $IC \approx 0.0644$).
4. Partition ciphertext into $m = 14$ cosets using `split_into_groups()`.
5. For each coset $j \in [0, m-1]$:
   1. Compute letter counts and percentages using `frequency_analysis()`.
   2. Find best Caesar shift using `find_shift()` by minimizing $\chi^2$ against English letter distribution.
6. Assemble recovered shifts into the keyword using `find_key()` $\to$ `AMBROISETHOMAS`.
7. Decrypt ciphertext using `vigenere_decrypt()` with the recovered keyword.
8. Re-encrypt plaintext using `vigenere_encrypt()` and execute `verify()` to ensure byte-exact match with preprocessed ciphertext (`PASS`).

## Important Commands
```bash
# Compile cryptanalysis modules
g++ -std=c++17 -Wall -Wextra -Wpedantic main.cpp kasiski.cpp frequency.cpp vigenere.cpp -o vigenere

# Execute attack on input ciphertext
./vigenere
```

## Observations
- Kasiski examination correctly isolated candidate factors, and Index of Coincidence confirmed the optimal key length of 14 ($IC \approx 0.0644$).
- Splitting the polyalphabetic ciphertext into 14 cosets effectively transformed it into 14 independent Caesar ciphers.
- Chi-square analysis accurately resolved each shift, recovering key `AMBROISETHOMAS`.
- Decrypted text yielded coherent English, and verification confirmed `PASS` with an exact re-encryption match.
