# 🔐 Security Policy

**QRMessenger Security Information and Vulnerability Reporting**

---

## Security Design

### ✅ What QRMessenger Does
- ✅ **Encrypts data locally** in your browser (zero trust)
- ✅ **Uses standard algorithms** (PBKDF2, AES-256-CTR)
- ✅ **No server communication** (works 100% offline)
- ✅ **No keys stored** (computed on-demand from password)
- ✅ **No tracking** (no analytics, no telemetry)

### ⚠️ What QRMessenger Does NOT Do
- ❌ **Authenticate messages** (no integrity checking; use v2.0 for that)
- ❌ **Verify sender identity** (symmetric encryption only)
- ❌ **Provide forward secrecy** (same password = same ciphertext)
- ❌ **Protect against key compromise** (if password is leaked, encryption is broken)
- ❌ **Offer asymmetric encryption** (no public-key cryptography)

---

## Cryptographic Foundation

### Algorithm Choices

**PBKDF2-SHA256 (Key Derivation)**
```
Password → [1M+ iterations] → 256-bit Key
```
- **Why**: Industry-standard, NIST-approved, proven
- **Iterations**: Configurable (1-32 digits = 1-100,000,000 iterations)
- **Security**: Slows brute-force attacks to ~0.1ms per guess
- **Trade-off**: Higher iterations = more secure but slower

**AES-256-CTR (Encryption)**
```
Plaintext + Key + IV → [CTR mode] → Ciphertext
```
- **Why**: Military-grade, NIST-approved, no padding needed
- **Key size**: 256 bits (computational security: ~128 bits effective)
- **Mode**: CTR (stream cipher, no authentication)
- **IV**: 16 random bytes per encryption (prevents pattern analysis)

### Security Properties

| Property | Level | Notes |
|----------|:-----:|-------|
| **Confidentiality** | ⭐⭐⭐⭐⭐ | AES-256 is strong |
| **Authenticity** | ⭐☆☆☆☆ | No HMAC; silent failure on wrong password |
| **Integrity** | ⭐☆☆☆☆ | CTR mode doesn't detect tampering |
| **Key Freshness** | ⭐⭐⭐ | Derived from password; no key exchange |
| **Forward Secrecy** | ⭐☆☆☆☆ | Same password = same ciphertext always |

---

## Threat Model

### Threats We Protect Against

| Threat | Protection | How |
|--------|:----------:|-----|
| **Eavesdropping** | ✅ Strong | AES-256 encryption |
| **Brute-Force** | ✅ Strong | PBKDF2 iterations slow password guessing |
| **Pattern Analysis** | ✅ Moderate | Random IV per encryption |
| **Replay Attack** | ⚠️ Weak | Same plaintext = same ciphertext (by design) |
| **Man-in-the-Middle** | ✅ Moderate | If password shared securely |
| **Tampering** | ❌ None | No authentication/integrity check |
| **Key Theft** | ❌ Critical | Password compromise = total compromise |

### Threats We DON'T Protect Against

| Threat | Why | Mitigation |
|--------|-----|-----------|
| **Compromised Device** | Malware can access plaintext/keys | Use trusted device only |
| **Compromised Browser** | Extensions can intercept data | Review browser extensions |
| **Weak Password** | Brute-force becomes trivial | Use strong passwords (16+ chars) |
| **Shared Password** | Anyone with password can read | Share password via different channel |
| **Lost Password** | No recovery mechanism | No backup (by design) |
| **Quantum Computing** | AES may become breakable | Monitor post-quantum research |

---

## Security Best Practices

### For Users

#### 1. **Use Strong Passwords**
```
❌ Weak:   "password", "123456", "qwerty", dictionary words
✅ Strong: "K7$mP9q!nR@2vX&wL4jYzB8", 16+ chars, mixed case/symbols/numbers
```
**Generation**: Use password manager (KeePass, 1Password, Bitwarden)

#### 2. **Never Share Password with Ciphertext**
```
❌ Bad:   "Here's encrypted message: ABC...XYZ, password is MyPass123"
✅ Good:  Send password via phone call / separate email / SMS
```

#### 3. **Use High Iteration Counts for Sensitive Data**
```
Low iterations (1,000): Fast but weak against brute-force
          Default (100,000): Good for general use
   High iterations (10,000,000): Very slow but very secure
```
**Recommendation**: Set iterations ≥ 100,000 for sensitive data

#### 4. **Test Recovery Before Relying**
```
1. Encrypt a test message
2. Close the app completely
3. Reopen and decrypt
4. Verify it works before using for critical data
```

#### 5. **Use Different Passwords for Different Messages**
```
❌ Bad:   Same password for 100 different messages
✅ Good:  Different password for each sensitive message (or groups)
```
**Why**: Pattern analysis becomes easier with same password

#### 6. **Assume Messages are Readable Once Decrypted**
```
❌ Bad:   Rely on encryption to prevent copying/screenshotting
✅ Good:  Treat decrypted message as readable plaintext (encrypt email too)
```

---

## Security for Developers

### If You Deploy QRMessenger

**✅ Do**:
- Serve over **HTTPS only** (GitHub Pages, Netlify do this)
- Use **CSP headers** to prevent injection:
  ```
  Content-Security-Policy: default-src 'self'; script-src 'self'
  ```
- **Monitor for vulnerabilities** in dependencies (jsQR, qrcode libs)
- Keep **browsers up-to-date** (WebCrypto API improvements)
- Enable **HSTS** (HTTP Strict Transport Security)

**❌ Don't**:
- Serve over plain HTTP (no HTTPS)
- Modify crypto code without understanding it
- Add tracking/analytics (defeats zero-trust model)
- Log plaintext or keys
- Cache encryption outputs
- Add "recover password" feature (breaks security)

### Code Review Checklist
```javascript
// ✅ Check random number generation
crypto.getRandomValues(iv)  // Good
Math.random() * 256         // WRONG!

// ✅ Check IV uniqueness
// Each encryption should have new random IV
// ❌ Reusing IV breaks security

// ✅ Check for logging
console.log(plaintext)      // WRONG!
console.log('encrypted ok') // Safe

// ✅ Check password handling
// Never log passwords
// Never store passwords in variables after use
```

---

## Known Limitations

### 1. **Silent Failure**
Wrong password doesn't produce error message; outputs garbage instead.

**Why**: Intentional (prevents leaking that password was wrong)  
**User Impact**: Users must know the correct password (or figure it out)

### 2. **No Message Authentication**
Data can be tampered with. Recipient won't know it was modified.

**Example**: Attacker can flip bits → recipient gets garbage  
**Why**: CTR mode is stream cipher (simpler, faster)  
**Mitigation**: Use v2.0 (has dual-layer redundancy)

### 3. **Deterministic Encryption**
Same plaintext + password + iterations = same ciphertext always.

**Why**: IV is deterministic (no random component)  
**Impact**: Patterns can leak data with large corpus  
**Mitigation**: Add random salt to plaintext (manually)

### 4. **No Forward Secrecy**
If password is compromised, all historical messages can be decrypted.

**Why**: Symmetric encryption (no ephemeral keys)  
**Mitigation**: Use different passwords for different messages

### 5. **Browser-Based Only**
Encrypted data visible in browser memory during decryption.

**Why**: Running in browser (not isolated)  
**Mitigation**: Use on trusted device only; close browser after

---

## Vulnerability Disclosure

### How to Report

**DO NOT** post vulnerabilities publicly on Issues/Discussions.

#### For Non-Critical Issues
1. Open a **private Security Advisory**:
   - Go to Security tab
   - Click "Report a vulnerability"
2. Describe issue (avoid public details)
3. Wait for maintainer response

#### For Critical Issues (RCE, Key Extraction)
1. Email maintainer directly:
   ```
   Subject: [SECURITY] QRMessenger Critical Vulnerability
   ```
2. Include:
   - Vulnerability description
   - Impact assessment
   - Proof of concept (if safe)
   - Recommended fix
3. Allow 90 days for remediation before public disclosure

### What Qualifies as Security Issue

**YES, Report**:
- ✅ Encryption is broken/bypassable
- ✅ Keys leak to browser console/network
- ✅ Passwords sent to servers
- ✅ XSS vulnerabilities
- ✅ CSRF attacks possible
- ✅ Service worker vulnerability

**Maybe, Discuss First**:
- ⚠️ Better crypto algorithms available
- ⚠️ UI improvements (copy button visibility)
- ⚠️ Password strength meter request

**NO, Don't Report**:
- ❌ "Use Argon2id instead of PBKDF2" (design choice)
- ❌ "Why no asymmetric encryption?" (scope)
- ❌ "Help me decrypt someone else's message" (threat model)

---

## Incident Response

If a vulnerability is discovered:

1. **Assessment** (24 hours)
   - Confirm impact and severity
   - Determine affected versions

2. **Fix Development** (48-72 hours)
   - Code fix created
   - Tests written
   - Peer reviewed

3. **Patch Release** (1-2 weeks)
   - New version released
   - Security advisory published
   - Users notified

4. **Post-Mortem** (ongoing)
   - Root cause analysis
   - Prevention measures
   - Documentation updated

---

## Compliance & Standards

### Standards Followed
- ✅ **NIST SP 800-132**: PBKDF2 recommendations
- ✅ **FIPS 197**: AES specification
- ✅ **RFC 3394**: Key derivation
- ✅ **RFC 3610**: AES-CCM mode guidance

### NOT Compliance Claims
- ❌ Not HIPAA-compliant (no audit logging)
- ❌ Not PCI-DSS-compliant (no key management system)
- ❌ Not SOC 2-certified (no formal audit)
- ⚠️ **Compliance**: User responsibility to use correctly

---

## Security Updates

### Version History
- **v1.0**: Current (PBKDF2+AES single-layer)
- **v2.0** (Archive): Dual-layer with Argon2id

### Update Policy
- Security fixes: Released ASAP (patch version)
- Feature updates: Regular releases (minor version)
- Major rewrites: Rare (breaking changes)

### How to Stay Updated
```bash
# Watch repository for releases
# Enable GitHub notifications
# Check Releases tab regularly
# Upgrade when new version available
```

---

## Security Contact

- **Email**: [maintainer@example.com](mailto:maintainer@example.com)
- **GitHub Issues**: Use private advisory (Security tab)
- **GPG Key**: [If applicable]

---

## Acknowledgments

Security contributions by:
- Community members reporting vulnerabilities
- Open-source projects (jsQR, qrcode)
- NIST and cryptography research community

---

## Disclaimer

QRMessenger is provided **as-is** for educational and legitimate communication purposes. Users are responsible for:

- Compliance with encryption laws in their country
- Choosing strong passwords
- Understanding security limitations
- Protecting devices from compromise
- Not using for illegal purposes

The maintainers are **NOT liable** for:
- Lost passwords or encrypted data
- Unauthorized decryption by others
- Misuse of the tool
- Incompatibility with browsers/devices

---

## FAQ

**Q: Is AES-256 really unbreakable?**  
A: Yes, currently. No known practical attacks. Estimated breakable time: >1 billion years with current technology.

**Q: Should I use v1.0 or v2.0?**  
A: Use **v1.0** (current). v2.0 archived only (overkill for most uses).

**Q: Can you decrypt my messages?**  
A: No. Only you (with password) can decrypt.

**Q: What if my password is weak?**  
A: Weak passwords can be brute-forced quickly. Use strong, random passwords.

**Q: Is this quantum-resistant?**  
A: No. Quantum computers could break AES/PBKDF2. Monitor post-quantum research (NIST PQC standards).

**Q: Can I use this for state secrets?**  
A: Only with full security review and high iteration counts. Use v2.0 for maximum security.

---

**QRMessenger Security Policy v1.0**  
Last updated: 2024

For questions: Open [GitHub Discussion](https://github.com/ggg343663/QRMessenger/discussions)

