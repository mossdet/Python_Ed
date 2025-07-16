# Python Super-Advanced 4.1: Security and Cryptography

## Table of Contents
- [Introduction](#introduction)
- [Cryptographic Implementations](#cryptographic-implementations)
- [Security Auditing and Penetration Testing](#security-auditing-and-penetration-testing)
- [Secure Coding Practices](#secure-coding-practices)
- [OAuth and JWT Deep Dive](#oauth-and-jwt-deep-dive)
- [Blockchain and Cryptocurrency Applications](#blockchain-and-cryptocurrency-applications)
- [Review Questions](#review-questions)
- [Answers to Review Questions](#answers-to-review-questions)

---

## Introduction
This module covers advanced security and cryptography topics for Python developers, including secure coding, cryptographic primitives, authentication protocols, and blockchain applications. Security is critical for protecting data, systems, and users in modern software.

## Cryptographic Implementations
- Use libraries like `cryptography`, `PyNaCl`, and `hashlib` for secure cryptographic operations.
- Symmetric encryption: AES, ChaCha20
- Asymmetric encryption: RSA, ECC
- Hashing: SHA-2, SHA-3, bcrypt, scrypt, Argon2
- Digital signatures and certificates
- Example: Encrypting data with Fernet (symmetric encryption)
  ```python
  from cryptography.fernet import Fernet
  key = Fernet.generate_key()
  f = Fernet(key)
  token = f.encrypt(b"secret data")
  data = f.decrypt(token)
  ```

## Security Auditing and Penetration Testing
- Use tools like Bandit, Safety, and PyLint for static analysis.
- Perform dependency vulnerability scans (e.g., `pip-audit`).
- Penetration testing with frameworks like OWASP ZAP, Burp Suite.
- Threat modeling and risk assessment.

## Secure Coding Practices
- Validate and sanitize all user input.
- Use parameterized queries to prevent SQL injection.
- Avoid hardcoding secrets; use environment variables or secret managers.
- Implement proper error handling and logging (avoid leaking sensitive info).
- Keep dependencies up to date and monitor for vulnerabilities.

## OAuth and JWT Deep Dive
- OAuth2: Authorization framework for delegated access.
- JWT (JSON Web Token): Compact, URL-safe means of representing claims.
- Use libraries: `authlib`, `PyJWT`, `django-oauth-toolkit`, `flask-jwt-extended`.
- Validate tokens, check expiration, and use secure signing algorithms.
- Example: Decoding a JWT
  ```python
  import jwt
  token = jwt.encode({'user_id': 123}, 'secret', algorithm='HS256')
  data = jwt.decode(token, 'secret', algorithms=['HS256'])
  ```

## Blockchain and Cryptocurrency Applications
- Use Python for smart contract development (e.g., Ethereum with web3.py).
- Build cryptocurrency wallets, trading bots, and blockchain explorers.
- Understand consensus algorithms, cryptographic proofs, and distributed ledgers.
- Security considerations: key management, transaction signing, replay attacks.

## Review Questions
1. What are the main cryptographic primitives used in Python?
2. How can you audit Python code for security vulnerabilities?
3. What are best practices for secure coding in Python?
4. Explain the OAuth2 flow and the role of JWTs.
5. What are some security considerations in blockchain applications?

## Answers to Review Questions
1. Symmetric (AES, ChaCha20), asymmetric (RSA, ECC), hashing (SHA-2, bcrypt), and digital signatures are key primitives.
2. Use static analysis tools (Bandit, Safety), dependency scans, and penetration testing frameworks.
3. Validate input, use parameterized queries, avoid hardcoded secrets, handle errors securely, and keep dependencies updated.
4. OAuth2 allows delegated access via tokens; JWTs are used to represent claims and must be validated for integrity and expiration.
5. Blockchain security includes key management, secure transaction signing, and protection against replay and consensus attacks.
