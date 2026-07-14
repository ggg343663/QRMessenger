# QRMessenger v2.0 (Pro) - Archive

**Dual-layer encryption system for high-security applications**

This folder contains QRMessenger v2.0, which is archived and not recommended for general use but preserved for compatibility and high-security projects.

---

## Why v1.0 is Recommended (Not v2.0)

### v1.0 Advantages
- ✅ **Simpler**: Single-layer PBKDF2+AES (easier to audit, verify, update)
- ✅ **Faster**: No extra key derivation; encryption/decryption in <1 second
- ✅ **Stable**: Less code = fewer bugs
- ✅ **Compatible**: Works across all devices/browsers consistently
- ✅ **Sufficient**: For normal secure messaging, PBKDF2+AES is industry-standard

### v2.0 Limitations (Why Archived)
- ❌ **Complex**: Dual-layer with Argon2id adds complexity
- ❌ **Slower**: Extra key derivation adds 1-2 seconds per operation
- ❌ **Maintenance Burden**: More code to maintain, debug, update
- ❌ **Overkill**: Double encryption rarely necessary for typical use
- ⚠️ **Silent Failure**: v2.0's "quiet failure on wrong password" can be confusing

---

## When to Use v2.0

Use `QRMessenger_v1.0(2.0).html` **only if**:

1. **Government/Military-Grade Encryption Required**: Double-layer encryption with memory-hard KDF
2. **Maximum Paranoia**: Redundant encryption layers reduce single-point-of-failure risk
3. **Legacy Compatibility**: Existing encrypted messages were created with v2.0
4. **High-Value Assets**: Protecting state secrets, advanced research, financial records

---

## Architecture Differences

### v1.0 (Recommended)
```
Password → PBKDF2-SHA256 (configurable rounds) → 256-bit Key
Key + Random IV → AES-256-CTR → Ciphertext
Output: Base64(IV + Ciphertext)
```

### v2.0 (Archived)
```
Password + Secret → Argon2id (memory-hard) → 256-bit Key
Key + Random IV1 → AES-256-CTR → Intermediate
Intermediate + Secret → PBKDF2-SHA256 → 256-bit Key2
Key2 + Random IV2 → AES-256-CTR → Final Ciphertext
Output: Base64(IV1 + IV2 + Ciphertext)
```

**Result**: v2.0 adds ~2 seconds per operation for minimal practical security gain over v1.0.

---

## Features of v2.0

- **Dual Encryption Layer**: Two independent AES-256-CTR passes
- **Memory-Hard KDF**: Argon2id (slows GPU/ASIC brute-force attacks)
- **Secret Field**: Optional second credential for enhanced security
- **Silent Failure**: Wrong password produces garbage (no error message)
- **Backward Compatible**: Can decode v1.0 messages (but not vice versa)

---

## How to Migrate from v2.0 to v1.0

### Option 1: Decode Everything with v2.0, Re-encode with v1.0
```
1. Open QRMessenger_v1.0(2.0).html in browser
2. Paste v2.0-encrypted message
3. Click Decode (⌛ Wait...)
4. Copy plaintext
5. Switch to QRMessenger_v1.0.html
6. Paste plaintext, set password, encode
7. Share new v1.0 encrypted message
```

### Option 2: Archive v2.0 Messages
- Keep v2.0-encrypted messages in archive folder
- Reference decryption method in README
- Use v1.0 for all new messages going forward

---

## Security Notes

### v2.0 Strengths
- Argon2id is **resistant to GPU/ASIC brute-force** (memory-hard)
- Dual encryption provides **redundancy** (one layer compromise ≠ full compromise)
- Extra obfuscation makes pattern analysis harder

### v2.0 Weaknesses
- **Complexity burden**: More code to audit, more bugs possible
- **Performance**: Slower decryption is poor UX (users may skip steps)
- **Marginal gain**: PBKDF2 with high rounds is already very secure
- **No authentication**: Still vulnerable to tampering (v1.0 also)

---

## Files in This Archive

- `QRMessenger_v1.0(2.0).html` — v2.0 full app with dual-layer encryption
- `README.md` — This file

---

## Contact / Support

For questions about v2.0 migration or dual-encryption setup:
- See main repository README
- Check GitHub Issues

---

**Recommendation**: Unless you specifically need dual-layer encryption, use **v1.0** for better performance, maintainability, and compatibility.

