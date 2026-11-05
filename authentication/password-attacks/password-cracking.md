# Password Hashing and Offline Cracking

An online password attack asks an application to verify candidate credentials. An offline password attack occurs when an attacker has obtained password verifiers, typically from a data breach, and can test guesses without contacting the login service. This distinction matters because online rate limits do not protect a stolen hash database.

## Hashing is not encryption

Encryption is designed to be reversible by a party with the correct key. A password hash is a one-way verifier: the application hashes a submitted password using the stored parameters and compares the result. Cracking a hash means testing candidate passwords against that verifier; it is not decrypting the hash.

Password hashes are intentionally expensive to compute. A **salt** is a unique, non-secret value used with each password so identical passwords do not yield identical stored hashes and precomputed tables are less useful. A **pepper** is an additional secret held separately from the password database, typically in a secrets-management boundary. A pepper can add protection if the database alone is exposed, but it does not replace per-password salts or a suitable password-hashing function.

## Storage choices

Fast general-purpose hashes such as SHA-256 are designed for throughput and are unsuitable by themselves for password storage: attackers can test many candidate passwords quickly. Use a dedicated password-hashing algorithm with a unique salt and a cost configured for the service:

| Algorithm | Notes |
|---|---|
| Argon2id | Memory-hard and generally a strong modern default when available and properly configured. |
| scrypt | Memory-hard; useful where supported and configured with appropriate cost parameters. |
| bcrypt | Widely supported adaptive password hash; account for its input-length behavior and tune its work factor. |
| PBKDF2 | Iterated key derivation with broad platform support; use current, appropriately high work factors and a strong hash variant. |

Choose parameters based on current platform guidance and measured server capacity, then revisit them over time. Do not invent a universal cost value. Use a vetted library and its password-hashing API rather than designing a custom construction.

## Protecting stored verifiers

Restrict database access, encrypt backups and transport, limit exposure of password records, and keep any pepper outside the database. Plan to rehash on successful authentication when parameters change. A breach response should consider forced resets, token/session invalidation, and user notification based on the incident.

Even a well-hashed password can be guessed offline if it is common, short, or reused. Unique long passwords, password managers, MFA, and breached-password screening reduce risk but do not eliminate the need for safe storage.

## Completed lab

**Offline password cracking** is completed. Its learning objective is to understand why exposed password verifiers and weak passwords remain risky. This note intentionally does not treat the exercise as proof that hashes are decryptable or that all cracking outcomes are predictable.

Related notes: [password attack patterns](./notes.md) and the [lab tracker](../lab-tracker.md).
