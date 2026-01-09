# Security Policy

## 🔒 Security Commitment

The Breach Around project takes security seriously. As a tool designed for security professionals, we hold ourselves to high standards for:

- Protecting user data and credentials
- Secure handling of API keys and tokens
- Responsible vulnerability disclosure
- Maintaining operational security (OPSEC)

---

## 🛡️ Supported Versions

We provide security updates for the following versions:

| Version | Supported          | End of Support |
| ------- | ------------------ | -------------- |
| 0.1.x   | ✅ Yes             | TBD            |
| < 0.1   | ❌ No              | Ended          |

**Recommendation**: Always use the latest version for the best security posture.

---

## 🚨 Reporting a Vulnerability

### What to Report

Please report any security vulnerabilities including:

- Authentication or authorization bypasses
- Credential leakage or exposure
- API key or token exposure
- Code injection vulnerabilities
- Dependency vulnerabilities (critical/high severity)
- Privacy violations
- Insecure default configurations

### What NOT to Report

Please do NOT report as security issues:

- Missing security headers in documentation
- Theoretical attacks without proof of concept
- Issues in unsupported versions
- Social engineering concerns
- Issues requiring physical access
- Spam or denial of service on public APIs (report to API provider)

---

## 📧 How to Report

### Step 1: Private Disclosure

**DO NOT create a public GitHub issue for security vulnerabilities!**

Instead, report privately using one of these methods:

#### Option A: GitHub Security Advisories (Recommended)

1. Go to the [Security tab](https://github.com/Fused-Gaming/breach-around/security)
2. Click "Report a vulnerability"
3. Fill out the advisory form with details
4. Submit the report

#### Option B: Email

Send an email to: **security@fusedgaming.com** (or your preferred contact)

**Email Template:**
```
Subject: [SECURITY] Breach Around Vulnerability Report

Vulnerability Type: [e.g., Credential Exposure]
Severity: [Critical/High/Medium/Low]
Affected Version(s): [e.g., 0.1.0]
Attack Vector: [e.g., Local/Network/Adjacent]

Description:
[Detailed description of the vulnerability]

Steps to Reproduce:
1. [Step 1]
2. [Step 2]
3. [Step 3]

Impact:
[What can an attacker achieve?]

Proof of Concept:
[Code or screenshots demonstrating the issue]

Suggested Fix:
[If you have ideas for fixing it]

Your Information:
Name: [Your name or alias]
Contact: [Email or GitHub username]
Public Credit: [Yes/No - Do you want to be credited publicly?]
```

### Step 2: Wait for Acknowledgment

- We will acknowledge receipt within **48 hours**
- We will provide an initial assessment within **7 days**
- We will keep you updated on progress

### Step 3: Coordinated Disclosure

- We will work with you to understand and reproduce the issue
- We will develop and test a fix
- We will coordinate a disclosure timeline (typically 90 days)
- We will credit you in release notes (if desired)

---

## 🔐 Security Features

### Data Protection

- **No Persistent Storage**: Credentials are never stored on disk
- **Memory Sanitization**: Sensitive data is cleared from memory after use
- **No Logging**: Passwords are never logged (even in debug mode)
- **Encrypted Transit**: All API calls use HTTPS/TLS

### API Security

- **k-Anonymity**: HIBP password checks use k-anonymity (passwords never sent)
- **Rate Limiting**: Built-in rate limiting prevents abuse
- **Token Management**: API keys are loaded from environment variables
- **Timeout Protection**: All network requests have timeouts

### Operational Security

- **Public Release Vehicle**: This repo contains sanitized code only
- **Private Development**: Development happens in private repository
- **Automated Sync**: Code is automatically sanitized before public release
- **No Secrets in Code**: All sensitive configuration uses environment variables

---

## 🛠️ Security Best Practices for Users

### For End Users

1. **Keep Updated**: Always use the latest version
   ```bash
   pip install --upgrade breach-around
   ```

2. **Secure API Keys**: Store keys in `.env` files, never in code
   ```bash
   # Good
   HIBP_API_KEY=your_key_here

   # Bad - don't commit keys!
   git add .env
   ```

3. **Validate Input**: Only check credentials you're authorized to test

4. **Secure Results**: Store results in encrypted locations or delete after use

5. **Network Security**: Use VPN or secure networks when running checks

### For Developers

1. **Code Review**: All code changes should be reviewed for security issues

2. **Dependency Scanning**: Run `pip audit` regularly
   ```bash
   pip install pip-audit
   pip-audit
   ```

3. **Static Analysis**: Use security linters
   ```bash
   pip install bandit
   bandit -r breach_around/
   ```

4. **Secret Scanning**: Never commit secrets
   ```bash
   git secrets --scan
   ```

5. **Least Privilege**: Request only necessary permissions

---

## 🔄 Security Update Process

### How We Handle Security Issues

1. **Verification**: We verify the reported vulnerability
2. **Assessment**: We assess severity using CVSS v3.1
3. **Fix Development**: We develop a fix in private
4. **Testing**: We thoroughly test the fix
5. **Release**: We release a security update
6. **Disclosure**: We publish a security advisory

### Severity Ratings

We use CVSS v3.1 for severity ratings:

| Score | Rating | Response Time |
|-------|--------|---------------|
| 9.0-10.0 | Critical | 24-48 hours |
| 7.0-8.9 | High | 7 days |
| 4.0-6.9 | Medium | 30 days |
| 0.1-3.9 | Low | 90 days |

### Security Advisories

Published advisories can be found at:
- [GitHub Security Advisories](https://github.com/Fused-Gaming/breach-around/security/advisories)

---

## 🏆 Hall of Fame

We recognize and thank security researchers who help improve Breach Around:

### 2026

- [No reports yet - be the first!]

### Recognition Criteria

To be listed in our Hall of Fame:
- Report must be a valid security vulnerability
- Report must follow responsible disclosure
- Report must be submitted before public disclosure
- Researcher must agree to be publicly credited

---

## 📚 Security Resources

### For Researchers

- [OWASP Top 10](https://owasp.org/www-project-top-ten/)
- [CWE - Common Weakness Enumeration](https://cwe.mitre.org/)
- [CVE - Common Vulnerabilities and Exposures](https://cve.mitre.org/)
- [CVSS Calculator](https://www.first.org/cvss/calculator/3.1)

### Security Tools

Recommended tools for security testing:

```bash
# Dependency vulnerability scanning
pip install pip-audit
pip-audit

# Static security analysis
pip install bandit
bandit -r breach_around/

# Secret detection
pip install detect-secrets
detect-secrets scan

# SAST scanning
pip install semgrep
semgrep --config=auto .
```

---

## ⚖️ Legal Protection

### Safe Harbor

We support safe harbor for security researchers who:

- Make a good faith effort to comply with this policy
- Do not access, modify, or delete user data
- Do not intentionally harm the system or users
- Do not publicly disclose vulnerabilities before coordinated disclosure
- Do not demand payment or compensation for disclosure

### Legal Safe Harbor Statement

We will not pursue legal action against researchers who:
1. Follow this security policy
2. Act in good faith
3. Do not violate laws or terms of service
4. Coordinate disclosure responsibly

---

## 🔄 Policy Updates

This security policy may be updated periodically. Significant changes will be:

- Announced in release notes
- Posted in repository discussions
- Highlighted in README

**Last Updated**: 2026-01-09
**Policy Version**: 1.0.0

---

## 📞 Contact

- **Security Email**: security@fusedgaming.com
- **GitHub Security**: [Report a vulnerability](https://github.com/Fused-Gaming/breach-around/security/advisories/new)
- **General Issues**: [GitHub Issues](https://github.com/Fused-Gaming/breach-around/issues) (non-security only)

---

## 🙏 Thank You

Thank you for helping keep Breach Around and its users safe!

Security is a community effort, and we appreciate responsible researchers who help us identify and fix vulnerabilities before they can be exploited.

---

**Remember**: If in doubt, report it privately first. We're here to work with you, not against you.
