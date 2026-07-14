# 📝 Changelog

**Version history and release notes for QRMessenger**

---

## Format

This changelog follows [Keep a Changelog](https://keepachangelog.com/) standards.

### Release Sections
- **Added**: New features
- **Changed**: Changes to existing functionality
- **Deprecated**: Features planned for removal
- **Removed**: Deleted features
- **Fixed**: Bug fixes
- **Security**: Security fixes/improvements

---

## [1.0] - 2024-01-xx (Current)

### Added
- ✅ **Web App Core**
  - Encode/Decode interface with tabbed navigation
  - Format selection: Text, 1-Way (PBKDF2), 2-Way (AES+PBKDF2)
  - Customizable iteration count (1-32 digits)
  - Real-time character counter

- ✅ **Encryption**
  - PBKDF2-SHA256 key derivation (industry-standard)
  - AES-256-CTR symmetric encryption
  - Segmented hashing for large iteration counts
  - Silent failure on wrong password (no error leakage)

- ✅ **QR Features**
  - Generate QR codes from encrypted output
  - Full-screen QR preview
  - QR code download as PNG
  - Import QR codes from image files
  - Import encrypted text from files

- ✅ **User Interface**
  - Dark theme (background: #0f172a, accent: #3b82f6)
  - Wait indicator (⌛ button) during processing
  - Success/error feedback (✓/✗)
  - Copy to clipboard functionality
  - Export as .txt file
  - Mobile-responsive design

- ✅ **PWA Support**
  - Service worker for offline functionality
  - Install as native app
  - Offline-first architecture
  - manifest.json with app metadata
  - Support for all modern browsers

- ✅ **Security**
  - Zero-trust: All encryption client-side
  - No server communication
  - No tracking or telemetry
  - HTTPS-ready (GitHub Pages, Netlify, Vercel compatible)

### Changed
- N/A (Initial release)

### Deprecated
- N/A

### Removed
- N/A

### Fixed
- N/A

### Security
- Proper use of Web Crypto API
- PBKDF2 with high iteration counts
- Random IV per encryption
- No hardcoded keys or secrets

---

## [1.0 (Pro v2.0)] - 2024-01-xx (Archive)

### Added (v2.0 Features)
- ✅ **Dual-Layer Encryption**
  - Secondary Secret field for enhanced security
  - Argon2id memory-hard KDF (GPU/ASIC resistant)
  - Two independent AES-256-CTR passes
  - Dual IV pairs for redundancy

- ✅ **Enhanced Security**
  - Argon2id (memory-hard key derivation)
  - Additional obfuscation layer
  - Pattern resistance improvement

### Changed (from v1.0)
- More complex encryption pipeline
- Longer processing time (~2 seconds per operation)
- Requires Secret field (in addition to password)

### Deprecated
- v2.0 is now archived (v1.0 recommended for new projects)

### Removed
- Not actively maintained
- Kept for compatibility with existing v2.0-encrypted data

### Fixed
- N/A (Archive version)

### Security
- Dual encryption layer provides redundancy
- Argon2id resistant to specialized hardware attacks

---

## Planned for Future Releases

### Version 1.1 (Minor Update)
- [ ] Theme selector (light/dark mode toggle)
- [ ] Keyboard shortcuts (Ctrl+Enter to encode)
- [ ] Improved error messages
- [ ] Copy feedback notification ("Copied!")
- [ ] Settings page for default iteration count

### Version 1.2 (Enhancement)
- [ ] Batch encryption (multiple files)
- [ ] File upload limit indicator
- [ ] Password strength meter
- [ ] Recent messages history (optional, local storage)
- [ ] Import/export settings

### Version 2.0+ (Future)
- [ ] Authentication support (email/username)
- [ ] Cloud sync (optional, encrypted)
- [ ] Message expiration (auto-delete)
- [ ] Sharing links with QRMessenger
- [ ] Browser extension
- [ ] Desktop app (Electron)

### Not Planned
- ❌ **Asymmetric encryption** (public-key; defeats symmetric design)
- ❌ **Password recovery** (no backdoor by design)
- ❌ **Server storage** (breaks zero-trust model)
- ❌ **Analytics/Tracking** (privacy-first)
- ❌ **Account system** (no accounts, no tracking)

---

## Security Updates

### Urgent (Patch Immediately)
- Critical vulnerability discoveries
- Encryption bypass
- Key extraction

### Important (Patch within 1 month)
- High-severity bugs
- Security hardening
- Algorithm updates

### Regular (Patch as available)
- Minor security improvements
- Best practice updates
- Dependency updates

---

## Version Naming

- **Major**: Breaking changes (1.0 → 2.0)
- **Minor**: New features backward-compatible (1.0 → 1.1)
- **Patch**: Bug fixes, security patches (1.0.0 → 1.0.1)

Format: `MAJOR.MINOR.PATCH` (e.g., 1.0.0)

---

## How to Check Version

1. **In app**: Usually in UI footer or "About"
2. **In source**: Check `QRMessenger_v1.0.html` version comments
3. **GitHub**: Check [Releases](https://github.com/ggg343663/QRMessenger/releases)

---

## Migration Guides

### Migrating from v2.0 to v1.0
```
1. Open QRMessenger_v1.0(2.0).html
2. Decode v2.0 messages to plaintext
3. Switch to QRMessenger_v1.0.html
4. Re-encode with new password
5. Share v1.0 encrypted messages
```

### Upgrading from older v1.0 to current
```
Backward compatible: All old v1.0 messages can be decoded
No special migration needed
```

---

## Contributing

Found an issue? Have a suggestion?
- Report: [GitHub Issues](https://github.com/ggg343663/QRMessenger/issues)
- Discuss: [GitHub Discussions](https://github.com/ggg343663/QRMessenger/discussions)
- Contribute: [CONTRIBUTING.md](./CONTRIBUTING.md)

---

## Release Schedule

- **Bugfix releases (patches)**: As needed (priority: security)
- **Feature releases (minor)**: Quarterly (if features requested)
- **Major releases**: Rare (only for significant redesigns)

---

## Archive Versions

### v1.1 (Archive)
- Segmented hashing attempt (v1.2-v1.3)
- Issue: Complex code, hard to verify
- Status: Archived for reference

### v1.3.3 (Archive)
- Web Worker approach (attempted)
- Issue: Performance improvement didn't justify complexity
- Status: Archived, not recommended

### v2.0 / v2.1 (Archive)
- Dual-layer encryption with Argon2id
- Issue: Overkill for most uses; ~2s per operation
- Status: Archived, preserved for compatibility

---

## End of Life (EOL)

When a version reaches EOL:
- No new features
- Security patches only for 12 months
- No technical support

**EOL Dates** (announced when applicable):
- v1.0: TBD (current, actively maintained)

---

## License

All versions licensed under MIT License.

See [LICENSE](./01_QRMessenger/LICENSE) for details.

---

## Feedback

- ⭐ Like the project? Star on GitHub
- 🐛 Found a bug? Report an issue
- 💡 Have an idea? Start a discussion
- 📝 Want to contribute? See [CONTRIBUTING.md](./CONTRIBUTING.md)

---

**Last Updated**: January 2024

