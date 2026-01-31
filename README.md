# Lab 2: Passwords

## Task 1: Basic Dictionary Attack

### 1. Describe what a basic dictionary attack is:
A basic dictionary attack is a password cracking technique that uses a pre-defined list of potential passwords (the "dictionary") and hashes them one by one. It then compares these generated hashes against the target hash to find a match.
* **Pros:** Extremely fast and effective against weak, common passwords.
* **Cons:** Fails immediately if the password is not explicitly listed in the file (e.g., if the user adds a single extra character like "apple1" instead of "apple").

### 2. How many sha512 hashed passwords were you able to crack?
**24**

### 3. How many yescrypt hashed passwords were you able to crack?
**29**

### 4. Describe what would be needed to crack all hashes in the `/data/*.hashes` files:
To crack the remaining hashes, we would need:
1.  **A Larger Wordlist:** A dictionary like `rockyou.txt` (14 million words) to cover more real-world passwords.
2.  **Rules/Mangling:** A cracking engine feature that automatically modifies dictionary words (e.g., changing "password" to "P@ssword1!") to catch variations.
3.  **Brute Force:** For random strings not found in any dictionary, we would need to try every possible character combination, which requires significantly more computing power and time.

### 5. How many total hashes would be computed while performing the above attacks?
**158,500** total computations.
*(Calculation: 112 SHA hashes × 500 words + 205 Yescrypt hashes × 500 words)*

---

## Task 2: Analysis & Rockyou Attack

### 1. Comparison of Results
Using the massive `rockyou.txt` wordlist significantly improved our success rate compared to the small `500_passwords.txt` list, even though the attack was stopped early due to time constraints.

| Hash Type | Cracked (Task 1: 500 Words) | Cracked (Task 2: Rockyou) | Improvement |
| :--- | :--- | :--- | :--- |
| **SHA-512** | 24 | **54** | +30 passwords |
| **Yescrypt** | 29 | **50** | +21 passwords |

### 2. Algorithm Comparison (Speed & Difficulty)
During the attacks, I observed a massive difference in performance between the two algorithms:
* **SHA-512:** This is a "fast" hashing algorithm. Hashcat was able to process these at a very high speed because the algorithm is designed for efficiency.
* **Yescrypt:** This algorithm is significantly slower. It is "memory-hard," meaning it forces the computer to use RAM for every guess. This prevented the processor from rushing through millions of guesses per second, making it much more resistant to cracking.

### 3. Why are some passwords still uncracked?
Even with the larger wordlist, 160 Yescrypt hashes remain. This is due to:
* **Time Constraints:** I terminated the Yescrypt attack early. Because the algorithm is so slow, running the full 14-million word dictionary against all hashes would have taken an impractical amount of time (hours or days) on this hardware.
* **Complexity:** Many uncracked passwords likely contain random characters (e.g., `Xy9#mP2!`) or patterns not found in the `rockyou` dictionary.
* **Passphrases:** Long sentences used as passwords are resistant to dictionary attacks unless the specific phrase is in the file.
