# QRMessenger v1.0 🔐

**A secure, offline-first text encryption and QR code sharing utility**

---

## Features

### Core Encryption
- **PBKDF2-SHA256**: Industry-standard key derivation with configurable iterations (1-32 digits)
- **AES-256-CTR**: Military-grade symmetric encryption
- **Deterministic Key Stretching**: Secure password-based key expansion

### Text Processing Formats
- **Text**: Plain text passthrough (no encryption)
- **Base64**: Reversible encoding with multiple rounds
- **1-Way (PBKDF2)**: One-way hashing for verification/storage
- **2-Way (AES+PBKDF2)**: Full encryption with password protection

### QR Code Features
- Generate QR codes from encrypted output
- Save QR images to disk
- Full-screen QR preview
- Import QR codes from images
- Auto-decode encrypted messages

### Security
- ✅ **Zero-Trust Architecture**: All encryption happens in-browser, no data sent to servers
- ✅ **Offline First**: Works completely offline; no internet required
- ✅ **No Tracking**: No analytics, no telemetry
- ✅ **Progressive Web App (PWA)**: Install on mobile/desktop like a native app

---

## How to Use

### 1. **Encode (Encrypt) Text**
   - Select format: Text → 1-Way → 2-Way
   - For 2-Way (AES), set a password and confirm
   - Set derive iterations (affects security & speed)
   - Click **Generate** and wait ⌛
   - Output appears below; Copy or Export as .txt
   - Generate QR code to share easily

### 2. **Decode (Decrypt) Text**
   - Paste encrypted text or import from image/file
   - Select matching format and iterations
   - Enter password (for AES mode)
   - Click **Regenerate** and wait ⌛
   - Decrypted message appears below

### 3. **Share Securely**
   - Export encrypted text as .txt file
   - Generate QR code for quick sharing
   - Share via email, messaging, or display on screen
   - Recipient opens QRMessenger and decodes with same password

---

## Security Considerations

### ✅ What This Provides
- **Symmetric encryption** (password-based)
- **Key stretching** to slow down brute-force attacks
- **Cross-platform compatibility** (any browser, any OS)
- **Full control** over encryption parameters

### ⚠️ Limitations
- **Single-layer by design** (lighter, faster, simpler than v2.0)
- **AES in CTR mode** (not authenticated; use for confidentiality, not integrity)
- **No forward secrecy** (same password always produces same ciphertext)
- **Offline only** (share via QR code, files, or manual copy-paste)

### 🔑 Best Practices
1. Use **strong, random passwords** (16+ characters, mixed case + symbols)
2. **Increase iterations** for important messages (↑ security, ↑ time)
3. Share password via **different channel** than ciphertext (never together)
4. **Test recovery** before relying on it for critical data
5. For **maximum security**, use v2.0 with dual-layer encryption

---

## Technical Architecture

### Encode Flow
```
Input Text
    ↓
[Format Selection]
    ├─ None        → Plain text (no processing)
    ├─ 1-Way       → PBKDF2-SHA256 hash (one-way, cannot reverse)
    └─ 2-Way       → PBKDF2 key derivation → AES-256-CTR encryption
         Password + Rounds → 256-bit key
         Random IV (16 bytes) + Ciphertext → Base64
```

### Decode Flow
```
Encrypted Input (Base64)
    ↓
[Decrypt with password & iterations]
    ├─ Extract IV from first 16 bytes
    ├─ Derive key from password + iterations
    ├─ AES-256-CTR decryption
    └─ Output plaintext (or garbage if wrong password)
```

### Key Derivation
- **Iterations parameter**: 1-32 digits (configurable security level)
- **Chained PBKDF2**: Four groups of up to 8 digits each
- **Silent Failure**: Decryption always succeeds (wrong password = garbage)

---

## Installation & Deployment

### Local Use (Desktop)
1. Download `QRMessenger_v1.0.html`
2. Open in browser: `file:///path/to/QRMessenger_v1.0.html`
3. Bookmark for quick access

### GitHub Pages
```bash
git clone <repo-url>
cd QRMessenger
# Push to GitHub Pages branch
git push origin main
# Access at: https://username.github.io/QRMessenger/
```

### PWA Installation
1. Open in modern browser (Chrome, Firefox, Safari, Edge)
2. Click install icon (address bar or menu)
3. Use like native app; works offline after first load

### Docker / Server
```dockerfile
FROM nginx:alpine
COPY QRMessenger_v1.0.html /usr/share/nginx/html/index.html
COPY manifest.json /usr/share/nginx/html/
EXPOSE 80
```

---

## Browser Compatibility

| Feature | Chrome | Firefox | Safari | Edge |
|---------|:------:|:-------:|:------:|:----:|
| AES-256-CTR | ✅ | ✅ | ✅ | ✅ |
| Web Crypto API | ✅ | ✅ | ✅ | ✅ |
| Service Worker | ✅ | ✅ | ⚠️ (Limited) | ✅ |
| QR Import | ✅ | ✅ | ✅ | ✅ |
| PWA Install | ✅ | ✅ | ✅ | ✅ |

**Min versions**: Chrome 37+, Firefox 34+, Safari 11+, Edge 79+

---

## File Structure
```
QRMessenger/
├── QRMessenger_v1.0.html        ← Main app (single-layer)
├── QRMessenger_v1.0(2.0).html   ← Archive (dual-layer, high-security)
├── manifest.json                ← PWA manifest
├── sw.js                        ← Service worker
├── icon-192.png                 ← App icon
├── icon-512.png                 ← App icon (large)
└── README.md                    ← This file
```

---

## Version History

### v1.0 (Current)
- Single-layer PBKDF2+AES encryption
- Fast, lightweight, deterministic
- Suitable for general messaging
- Cross-app compatible

### v1.0 (Pro v2.0) - Archive
- Dual-layer encryption (PBKDF2+Argon2id)
- Memory-hard key derivation
- Silent failure (security through obscurity)
- High-security messaging (confidential data)

---

## FAQ

**Q: Is this really offline?**  
A: Yes, 100%. All encryption/decryption happens in your browser using the Web Crypto API. No data is sent anywhere.

**Q: Can I decrypt on a different device?**  
A: Yes, as long as you have the same password and iterations setting. Use the QR code or export as .txt.

**Q: What if I forget the password?**  
A: Encrypted data is lost. There is no recovery mechanism (by design—no backup = no interception point).

**Q: How secure is PBKDF2+AES really?**  
A: As secure as your password. 1 million iterations adds ~0.1s to each decryption attempt, exponentially slowing brute-force. Recommended: 100,000+ iterations for high-value data.

**Q: Can you decrypt my data?**  
A: No. No keys are stored, no backdoors exist. Only you (with the password) can decrypt.

**Q: Why no authentication (HMAC/GCM)?**  
A: This version prioritizes simplicity and compatibility. v2.0 adds authentication via dual-layer structure.

---

## License

Open source. Free to use, modify, and distribute.

---

## Support

- Issues: GitHub Issues
- Suggestions: GitHub Discussions
- Security: Report privately to maintainer

---

**QRMessenger v1.0** — Encrypt, Share, Never Expose 🔐

