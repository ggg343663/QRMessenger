# QRMessenger 🔐

**End-to-end encryption and secure QR code sharing for modern messaging**

<div align="center">

![QRMessenger](https://img.shields.io/badge/QRMessenger-v1.0-blue?style=for-the-badge)
![License](https://img.shields.io/badge/License-MIT-green?style=for-the-badge)
![Browser Support](https://img.shields.io/badge/Browsers-All%20Modern-brightgreen?style=for-the-badge)

[🌐 Open Web App](#quick-start) • [📖 Full Documentation](./01_QRMessenger/README.md) • [🔐 Security](#security) • [📦 Archive](./archive)

</div>

---

## What is QRMessenger?

QRMessenger is a **zero-trust, offline-first encryption utility** that lets you:

✅ Encrypt messages using PBKDF2+AES-256 (military-grade)  
✅ Generate QR codes for easy sharing  
✅ Decode encrypted messages from images or text  
✅ Export encrypted data to .txt files  
✅ Work 100% offline (no servers, no tracking)  
✅ Use as progressive web app (PWA) on any device  

**In one sentence**: Take plaintext → encrypt it → get QR code → share safely.

---

## 🚀 Quick Start

### Option 1: Web Browser (Recommended)
1. Open [index.html](./01_QRMessenger/index.html) in your browser
2. Or download the HTML file and open locally

### Option 2: GitHub Pages
```bash
# Clone this repo
git clone https://github.com/ggg343663/QRMessenger.git
cd QRMessenger/01_QRMessenger

# Serve locally (if you have Python)
python -m http.server 8000
# Open: http://localhost:8000
```

### Option 3: Install as PWA
1. Open QRMessenger in Chrome/Edge/Firefox
2. Click the "Install" icon in address bar
3. Use like a native app (works offline after first load)

---

## 💻 Use Cases

| Use Case | Example |
|----------|---------|
| **Secure Messaging** | Send sensitive text via QR code without servers |
| **Password Storage** | Encrypt credentials as QR code for printing |
| **Document Sharing** | Encrypt large documents, share via QR code |
| **One-Time Pads** | Generate encryptions with high iteration count |
| **Mobile-First** | No app installation needed; works in browser |

---

## 🔐 Security

### What You Get
- **AES-256-CTR**: Military-grade symmetric encryption
- **PBKDF2-SHA256**: 1M+ iterations for password-based keys
- **Zero-Knowledge**: No servers, all encryption happens in your browser
- **Open Source**: Audit the code yourself

### What You Don't Get
- **Asymmetric Encryption**: No public-key cryptography (not needed for symmetric keys)
- **Forward Secrecy**: Same password = same ciphertext (by design)
- **Message Authentication**: CTR mode doesn't verify data integrity (use v2.0 for that)

### Best Practices
1. Use **strong, random passwords** (16+ chars, mixed case + numbers + symbols)
2. **Share password separately** from ciphertext (different channel/method)
3. **Increase iterations** for highly sensitive data (↑ security, ↑ time)
4. **Never reuse passwords** across unrelated messages (different keys = less pattern leakage)

---

## 📁 Project Structure

```
QRMessenger/
├── 01_QRMessenger/                    ← MAIN APPLICATION
│   ├── QRMessenger_v1.0.html         ← Primary app (single-layer encryption)
│   ├── QRMessenger_v1.0(2.0).html    ← Archived v2.0 (dual-layer, optional)
│   ├── manifest.json                  ← PWA configuration
│   ├── sw.js                          ← Service worker (offline support)
│   ├── index.html                     ← Redirect to v1.0
│   ├── README.md                      ← Full documentation
│   └── LICENSE                        ← MIT License
│
├── archive/                           ← ARCHIVE & HISTORY
│   ├── 01_QRMessenger/               ← Old versions (v1.0, v1.1)
│   ├── 02_QRMessenger_Pro/           ← v2.0 versions (dual-layer)
│   └── README.md                      ← Archive documentation
│
├── picture/                           ← ASSETS
│   └── file_00000000a00c71fabaa9743846ddcc1f.png ← App icon
│
└── build/                             ← COMPILED EXECUTABLES (ignored)
    ├── AirdrobText/
    ├── AirdrobTool/
    ├── AirdropWalletTool/
    └── QRMassenger/
```

---

## 🎯 Encryption Modes

### 1. **Text** (No Encryption)
- Input → Output (1:1 passthrough)
- Use for: Testing, data preservation

### 2. **1-Way (PBKDF2 Hash)**
- Input → SHA256 digest (irreversible)
- Use for: Password verification, checksums, one-way storage

### 3. **2-Way (PBKDF2 + AES)**
- Input + Password → Encrypted output (reversible with password)
- Use for: Secure messaging, data confidentiality
- Supported formats: Text, Base64, etc.

---

## 🔧 Features

| Feature | v1.0 | v2.0 Archive |
|---------|:----:|:----------:|
| PBKDF2 Encryption | ✅ | ✅ |
| AES-256-CTR | ✅ | ✅ |
| Argon2id KDF | ❌ | ✅ |
| Dual-Layer Encryption | ❌ | ✅ |
| QR Generation | ✅ | ✅ |
| QR Import | ✅ | ✅ |
| File Export | ✅ | ✅ |
| Offline PWA | ✅ | ✅ |
| Wait Indicator | ✅ | ✅ |

---

## 📱 Browser Support

| Browser | Support | Notes |
|---------|:-------:|-------|
| Chrome | ✅ v37+ | Full support, recommended |
| Firefox | ✅ v34+ | Full support |
| Safari | ✅ v11+ | Full support (PWA limited) |
| Edge | ✅ v79+ | Full support |
| Opera | ✅ | Chromium-based, full support |

---

## 🚀 Deployment

### Self-Hosted
```bash
# Serve with any static server
npx http-server ./01_QRMessenger
# Or with Python
cd ./01_QRMessenger
python -m http.server 8000
```

### GitHub Pages
1. Fork this repository
2. Enable Pages in Settings
3. Select `main` branch, `/01_QRMessenger` folder
4. Access at: `https://username.github.io/QRMessenger/`

### Docker
```dockerfile
FROM nginx:alpine
COPY 01_QRMessenger/ /usr/share/nginx/html/
EXPOSE 80
```

---

## 🔍 How It Works

### Encoding (Encryption) Process
```
User Input
    ↓
[Choose Encryption Mode]
    ├─ Text → Output plaintext
    ├─ 1-Way → Hash with PBKDF2-SHA256
    └─ 2-Way → PBKDF2 key + AES-256-CTR
         ↓
    Generate Random 16-byte IV
         ↓
    Combine: IV + Ciphertext → Base64 Encode
         ↓
    Output: Shareable encrypted string
    Optional: Generate QR code from output
```

### Decoding (Decryption) Process
```
Encrypted Input (Base64)
    ↓
[Extract 16-byte IV]
    ↓
[Derive Key from Password + Iterations]
    ↓
[AES-256-CTR Decrypt: Ciphertext]
    ↓
Output: Original plaintext
Note: Wrong password = garbage (no error message)
```

---

## 🛡️ Security Model

### Threat Protection
| Threat | Protection | Notes |
|--------|-----------|-------|
| **Eavesdropping** | ✅ AES-256 | Network traffic encrypted |
| **Man-in-the-Middle** | ⚠️ Weak | Requires password sharing over safe channel |
| **Brute-Force** | ✅ PBKDF2 | Configurable iterations slow password guessing |
| **Replay Attack** | ❌ None | Same password = same ciphertext |
| **Tampering** | ❌ None | No HMAC/authentication (use v2.0 for that) |

### Assumptions
1. **Your browser is trusted** (not compromised with malware)
2. **Passwords are kept secret** (not shared insecurely)
3. **Iterations are set high enough** (1M+ recommended)

---

## ❓ FAQ

**Q: Is my data safe?**  
A: Yes. All encryption happens in your browser. We don't see anything.

**Q: What if I forget the password?**  
A: Lost forever. No recovery mechanism (intentional design).

**Q: Can I use the same password multiple times?**  
A: Yes, but it reduces entropy (use different passwords for different messages).

**Q: Why PBKDF2 and not Argon2id?**  
A: PBKDF2 is simpler, faster, and sufficient for most use cases. v2.0 includes Argon2id for high-security needs.

**Q: Does this work without internet?**  
A: Yes, 100%. Open once, then works offline forever.

**Q: Can you decrypt my messages?**  
A: No. The keys exist only on your device and are never transmitted.

**Q: Is this NIST-approved?**  
A: PBKDF2 and AES-256 are NIST standards. The implementation is straightforward, no exotic modes.

---

## 📜 License

MIT License — Free to use, modify, and distribute (see [LICENSE](./01_QRMessenger/LICENSE))

---

## 🔐 Disclaimer

This software is provided **as-is** for educational and legitimate communication purposes. Users are responsible for:

- Compliance with their country's encryption laws
- Choosing strong passwords
- Protecting their passwords from disclosure
- Understanding the security model

The authors are **not responsible** for:
- Lost passwords or encrypted data
- Misuse for illegal purposes
- Compatibility issues with browsers/devices
- Third-party interception of passwords

---

## 📮 Contact & Support

- **Issues**: [GitHub Issues](https://github.com/ggg343663/QRMessenger/issues)
- **Discussions**: [GitHub Discussions](https://github.com/ggg343663/QRMessenger/discussions)
- **Security Report**: [Private Security Issue](https://github.com/ggg343663/QRMessenger/security)

---

<div align="center">

**[⬆ back to top](#qrmessenger-)**

Made with 🔐 for secure communication  
[Fork on GitHub](https://github.com/ggg343663/QRMessenger) • [Open App](./01_QRMessenger/index.html)

</div>
