# 🤝 Contributing to QRMessenger

Thank you for your interest in contributing to QRMessenger! This document provides guidelines and instructions.

---

## Code of Conduct

- Be respectful and inclusive
- Focus on ideas, not on people
- Help create a welcoming environment
- Report inappropriate behavior to maintainers

---

## How to Contribute

### 1. Report Bugs
**Found a bug?** Open an [Issue](https://github.com/ggg343663/QRMessenger/issues)

Include:
- Browser and version
- Steps to reproduce
- Expected vs actual behavior
- Screenshots (if applicable)
- Console errors (F12 → Console)

```markdown
**Browser**: Chrome 120.0
**Steps**:
1. Enter text "test"
2. Click Generate
3. Observe: ...

**Expected**: ...
**Actual**: ...
```

### 2. Suggest Features
**Have an idea?** Open a [Discussion](https://github.com/ggg343663/QRMessenger/discussions)

Describe:
- What problem does it solve?
- How would it work?
- Is there an alternative?
- Any security implications?

### 3. Submit Code Changes

#### Prerequisites
- Git installed
- Text editor or VS Code
- Basic knowledge of HTML/JavaScript

#### Steps

**Fork the repository**
```bash
# Click "Fork" on GitHub
# Clone your fork
git clone https://github.com/YOUR-USERNAME/QRMessenger.git
cd QRMessenger
```

**Create a branch**
```bash
git checkout -b feature/your-feature-name
# or
git checkout -b fix/your-bug-fix
```

**Make your changes**
- Edit files as needed
- Test thoroughly in multiple browsers
- Keep changes focused and atomic

**Write meaningful commits**
```bash
git add .
git commit -m "Add feature: brief description

- Detailed explanation of what changed
- Why this change was necessary
- Any testing performed
"
```

**Push and submit PR**
```bash
git push origin feature/your-feature-name

# Go to GitHub and open a Pull Request
# Fill in the PR template completely
```

---

## Coding Standards

### JavaScript
- Use vanilla JS (no frameworks required)
- ES6+ syntax is fine (template literals, arrow functions, etc.)
- Add comments for complex crypto logic
- Follow existing code style

```javascript
// ✅ Good: Clear, documented
function derivePbkdfBits(rounds) {
  // Split rounds into 4 groups of 8 digits
  // Chain PBKDF2 between groups for key stretching
  const groups = [];
  // ... implementation ...
  return key;
}

// ❌ Avoid: Unclear
function deriveKey(r) {
  // Some processing...
  return Math.random() * r; // Wrong!
}
```

### HTML/CSS
- Semantic HTML5
- Mobile-responsive design
- Accessible (ARIA labels, keyboard navigation)
- Dark mode support

```html
<!-- ✅ Good -->
<button id="encode-btn" aria-label="Encode and encrypt">
  Generate
</button>

<!-- ❌ Avoid -->
<div onclick="encode()">Generate</div>
```

### Comments
- Explain *why*, not *what*
- Comment complex crypto operations
- Keep comments up-to-date with code

```javascript
// ✅ Good: Explains the decision
// Use CTR mode (not GCM) for simplicity:
// authentication not needed for confidentiality-only use case
const mode = 'aes-256-ctr';

// ❌ Avoid: Restates obvious code
const mode = 'aes-256-ctr'; // Set mode to aes-256-ctr
```

---

## Security Considerations

**QRMessenger handles encryption.** Please:

1. **Understand the crypto** before modifying:
   - PBKDF2-SHA256: Key derivation
   - AES-256-CTR: Symmetric encryption
   - Silent failure: Intentional design

2. **Do not**:
   - Add hardcoded keys or secrets
   - Change encryption without documenting it
   - Log plaintext or keys
   - Use weak RNG (use `crypto.getRandomValues()`)

3. **Do**:
   - Peer review security changes
   - Document any crypto modifications
   - Test with weak passwords (to verify it still works)
   - Validate all inputs

4. **Security fixes**: Report privately to maintainers
   ```
   Subject: [SECURITY] QRMessenger vulnerability
   ```

---

## Testing

### Browser Testing
Test on:
- ✅ Chrome (latest)
- ✅ Firefox (latest)
- ✅ Safari (latest)
- ✅ Edge (latest)
- ✅ Mobile browsers

### Feature Testing
Test each mode:
- [ ] Text mode (no encryption)
- [ ] 1-Way mode (PBKDF2 hash)
- [ ] 2-Way mode (AES+PBKDF2)
- [ ] QR generation
- [ ] QR import
- [ ] File import/export

### Edge Cases
- [ ] Empty input
- [ ] Very long input (1MB+)
- [ ] Special characters (emoji, unicode)
- [ ] Wrong password decryption
- [ ] Mobile keyboard input

---

## PR Template

Use this structure for pull requests:

```markdown
## Description
Brief explanation of changes

## Type of Change
- [ ] Bug fix
- [ ] New feature
- [ ] Documentation update
- [ ] Security improvement

## Related Issues
Closes #123

## Changes Made
- Change 1
- Change 2
- Change 3

## Testing Done
- Tested in Chrome v120
- Tested encoding/decoding
- Tested on mobile

## Screenshots (if applicable)
[Add images]

## Checklist
- [ ] Code follows style guidelines
- [ ] Self-review completed
- [ ] Comments added
- [ ] Documentation updated
- [ ] No breaking changes
- [ ] Tested in multiple browsers
```

---

## File Structure

```
QRMessenger/
├── 01_QRMessenger/
│   ├── QRMessenger_v1.0.html      ← Main app
│   ├── manifest.json               ← PWA config
│   ├── sw.js                       ← Service worker
│   └── README.md                   ← App documentation
├── README.md                       ← Project overview
├── DEPLOYMENT.md                   ← Deployment guide
├── CONTRIBUTING.md                 ← This file
└── archive/                        ← Old versions
```

### What to Edit

**Main Application Logic**:
- File: `01_QRMessenger/QRMessenger_v1.0.html`
- Look for: `<script>` section
- Functions: `buildPassword()`, `runDecodeText()`, `aesEncrypt()`, `aesDecrypt()`

**PWA/Offline Support**:
- Files: `manifest.json`, `sw.js`
- Controls: Installation, offline behavior

**Documentation**:
- Files: `README.md`, `DEPLOYMENT.md`
- Edit in Markdown format

---

## Common Contribution Types

### Bug Fix
```bash
git checkout -b fix/wrong-password-feedback
# Make changes to improve error handling
# Commit and push
```

### Feature Addition
```bash
git checkout -b feature/add-clipboard-copy
# Add button to copy output to clipboard
# Document in README
```

### Documentation Improvement
```bash
git checkout -b docs/clarify-password-format
# Update README with better examples
# Fix typos
```

### Performance Optimization
```bash
git checkout -b perf/faster-key-derivation
# Optimize crypto functions
# Benchmark before/after
```

---

## Development Tips

### Quick Testing
1. Edit `QRMessenger_v1.0.html`
2. Save
3. Refresh browser (F5)
4. Test in DevTools (F12)

### Using Browser DevTools
```javascript
// Console commands for testing
// Simulate encrypt
buildPassword();

// Simulate decrypt with wrong password
runDecodeText();

// Check performance
console.time('encrypt');
aesEncrypt('test', 'password', 100000, '');
console.timeEnd('encrypt');
```

### Debugging Crypto
```javascript
// Check key derivation
let key = derivePbkdfBits(1000);
console.log('Key hex:', Array.from(key).map(b => b.toString(16)).join(''));

// Check encryption output
let encrypted = aesEncrypt('test', 'pass', 100000, '');
console.log('Encrypted (base64):', encrypted);
```

---

## Commit Message Guidelines

Format:
```
<type>(<scope>): <subject>

<body>

<footer>
```

Types:
- `feat`: New feature
- `fix`: Bug fix
- `docs`: Documentation
- `style`: Code style (no functional change)
- `refactor`: Code reorganization
- `perf`: Performance improvement
- `test`: Testing updates
- `security`: Security fix

Examples:
```
feat(crypto): add Argon2id support for v2.0
fix(ui): correct wait button color during decode
docs(readme): improve security section clarity
perf(crypto): optimize PBKDF2 for faster key derivation
security(input): validate file size before import
```

---

## PR Review Process

1. **Automatic checks**:
   - Browser compatibility
   - Code style lint (if applicable)

2. **Maintainer review**:
   - Code quality
   - Security implications
   - Testing thoroughness

3. **Feedback**:
   - May request changes
   - May suggest improvements
   - Will approve or close

4. **Merge**:
   - PR merged to main
   - Contributor credited
   - Changes deployed automatically

---

## Recognition

Contributors will be:
- ✅ Added to CONTRIBUTORS.md
- ✅ Credited in commit message
- ✅ Mentioned in release notes
- ✅ Thanked publicly

---

## Questions?

- Open a [Discussion](https://github.com/ggg343663/QRMessenger/discussions)
- Review [existing Issues](https://github.com/ggg343663/QRMessenger/issues)
- Check [README](./README.md) and [DEPLOYMENT.md](./DEPLOYMENT.md)

---

## License

By contributing, you agree that your code is licensed under the MIT License (see [LICENSE](./01_QRMessenger/LICENSE)).

---

Thank you for helping make QRMessenger better! 🙏

