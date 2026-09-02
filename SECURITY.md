# Security policy

HashLib4Pascal is a hashing library providing checksums, non-cryptographic hashes, cryptographic
hashes, KDFs (Argon2, Scrypt, PBKDF2-HMAC), MACs (HMAC, KMAC, Blake2BMAC/Blake2SMAC), and XOFs. Several
of these (password hashing, MACs) are security-critical, so a correctness or memory-safety bug here can
have real impact. We take reports of these seriously and are grateful to anyone who reports them
responsibly.

## Supported versions

`master` is the actively maintained branch and the source for all releases. Fixes land on `master`
first and ship in the next tagged release, so `master` may already contain a fix that hasn't been
released yet — please check against `master` before reporting an issue. Older tagged releases are not
backported to.

## Reporting a vulnerability

**Please report security issues privately — do not open a public issue, pull request, or discussion
for a suspected vulnerability.**

Preferred channel: **GitHub private vulnerability reporting.** On this repository, go to the
**Security** tab → **Report a vulnerability**.

A good report includes:

- the affected algorithm/component (e.g. `Argon2id`, `THMAC`, `TBlake3`, the state-based/incremental
  API, the cloning mechanism) and version or commit;
- a clear description of the issue and its impact (crash, out-of-bounds read/write, incorrect digest
  or MAC, a timing side-channel, unbounded memory/CPU use, etc.);
- a minimal reproduction — input data, key/salt/parameters used, and expected vs. actual output — and
  the affected toolchain (Delphi or FreePascal, version, OS, architecture) where relevant;
- any suggested remediation, if you have one.

You do not need a working exploit — a credible analysis of a broken invariant (e.g. a non-constant-time
comparison where one is expected, or a KDF parameter that isn't applied correctly) is enough.

## What to expect

This is a solo-maintained open-source project, so responses are best-effort rather than covered by a
formal SLA. In general you can expect:

- **Acknowledgement** of your report, typically within a few days.
- An initial **assessment** (is it a vulnerability, likely severity, affected versions) once it's
  been reviewed.
- **Coordinated disclosure.** We aim to develop and release a fix before public disclosure, and to
  coordinate timing with you. Our default embargo target is **90 days** from the initial report,
  shorter for issues under active exploitation and extendable by mutual agreement for complex fixes.
- **Credit** in the release notes, if you'd like it. Let us know if you'd prefer to remain anonymous.

## Scope

**In scope** — issues in the implementations this repository ships:

- memory-safety bugs in any hash, MAC, KDF, or XOF implementation (out-of-bounds read/write, incorrect
  buffer-size calculations);
- an algorithm producing output that deviates from its specification/test vectors (wrong digest, MAC,
  or derived key for valid input);
- state-based (incremental) hashing producing a different result than the equivalent one-shot call, or
  cloned instances diverging incorrectly;
- non-constant-time comparison or other timing side-channels in MAC verification (HMAC, KMAC,
  Blake2BMAC/Blake2SMAC) or password-hash verification;
- incorrect handling of KDF parameters (Argon2 cost/parallelism/memory, Scrypt N/r/p, PBKDF2
  iterations) that silently weakens the derived key;
- secret material (keys, passwords, intermediate state) not cleared from memory where the API
  documents that it is;
- unbounded memory or CPU consumption when processing attacker-controlled input.

**Out of scope / report elsewhere:**

- The known cryptographic weaknesses of algorithms included for **legacy/compatibility** reasons (MD2,
  MD4, MD5, SHA-0, SHA-1, GOST, HAVAL, etc.) — offering them is documented and intentional. An
  implementation bug that makes one of them behave *incorrectly relative to its own spec* is still in
  scope.
- Using a non-cryptographic hash or checksum (CRC, Murmur, FNV, xxHash, etc.) for a security purpose —
  that is a misuse of the algorithm by the caller, not a vulnerability in this library.
- General bugs, incorrect documentation, or feature requests with no security impact — please use the
  normal [issue tracker](https://github.com/Xor-el/HashLib4Pascal/issues) for those.
- Issues in third-party code that merely uses this library.
