# 🛡️ Breach Around

**A comprehensive, multi-service breach database checker and OSINT toolkit for authorized security testing.**

[![PyPI version](https://badge.fury.io/py/breach-around.svg)](https://badge.fury.io/py/breach-around)
[![Python Versions](https://img.shields.io/pypi/pyversions/breach-around.svg)](https://pypi.org/project/breach-around/)
[![License](https://img.shields.io/github/license/Fused-Gaming/breach-around)](LICENSE)
[![GitHub Release](https://img.shields.io/github/v/release/Fused-Gaming/breach-around)](https://github.com/Fused-Gaming/breach-around/releases)

---

## 🚀 Quick Start

### Installation

```bash
# From PyPI (Recommended)
pip install breach-around

# From source
git clone https://github.com/Fused-Gaming/breach-around.git
cd breach-around
pip install -e .
```

### Basic Usage

```bash
# Check credentials from a CSV file
breach-checker check input.csv

# Check with specific services
breach-checker check input.csv --proxynova --hibp

# View results
breach-checker results latest
```

📖 **[Read the Quick Start Guide](QUICKSTART.md)** for detailed setup instructions.

---

## 🎯 Features

### ✅ Multi-Service Support
Check credentials against multiple breach databases simultaneously:
- **ProxyNova COMB** - Free, comprehensive breach database
- **Have I Been Pwned (HIBP)** - Industry-standard breach checking
- **LeakCheck.io** - Advanced breach intelligence

### ✅ Intelligent Analysis
- Aggregate results from multiple services
- Identify compromised passwords
- Track breach history per account
- Generate actionable security reports

### ✅ Professional Reporting
- **CSV** - Spreadsheet-ready summaries
- **JSON** - API-compatible structured data
- **Detailed Logs** - Complete investigation reports

### ✅ Enterprise Features
- Rate limiting and retry logic
- Concurrent checking (where supported)
- Robust error handling
- Comprehensive logging

---

## 🌐 Supported Services

| Service | Type | Rate Limit | Cost | Features |
|---------|------|-----------|------|----------|
| **ProxyNova COMB** | Public | ~1/sec | Free | Email & password breach lookup |
| **Have I Been Pwned** | Public/Paid | 1/1.5s | Free/Paid | Password check (free), Account lookup (paid) |
| **LeakCheck.io** | Paid | Custom | From $2 | Comprehensive breach database |

---

## 📋 Input Formats

The tool automatically detects and parses multiple CSV formats:

```csv
# Format 1: Simple email:password
email:password
user@example.com:Password123

# Format 2: Comma-separated
email,password
user@example.com,Password123

# Format 3: With metadata
email,pass,balance,value
user@example.com:Password123 | Balance = 50 | CertificateValue = 10
```

---

## 📊 Output Example

### CSV Summary
```csv
email,password,overall_status,total_breaches,password_compromised,breached_services
user@test.com,Pass123,breached,15,YES,"ProxyNova, HIBP"
safe@test.com,Secure456,clean,0,NO,""
```

### JSON Results
```json
{
  "email": "user@example.com",
  "overall_status": "breached",
  "total_breaches": 15,
  "password_compromised": true,
  "services": {
    "ProxyNova": { ... },
    "HIBP": { ... }
  }
}
```

---

## 🔐 Security & Privacy

### Password Protection
- HIBP uses k-anonymity - passwords are never sent to the server
- Local SHA-1 hashing before API calls
- No credential storage or logging of sensitive data

### API Key Security
- Store keys in `.env` files (gitignored)
- Support for environment variables
- Never commit credentials to repositories

---

## 🎨 Use Cases

### ✅ Authorized Security Testing
- Penetration testing engagements
- Security audits and assessments
- Incident response and breach validation
- Password policy enforcement

### ✅ Organizational Security
- Employee credential monitoring
- Third-party risk assessment
- Supply chain security validation
- Compliance verification

### ✅ Research & Education
- Security research projects
- Educational demonstrations
- CTF competitions
- Threat intelligence gathering

---

## 📚 Documentation

- 📖 **[Quick Start Guide](QUICKSTART.md)** - Get up and running in 5 minutes
- 🤝 **[Contributing Guidelines](CONTRIBUTING.md)** - Help improve the project
- ⚙️ **[Setup & Configuration](SETUP.md)** - Detailed setup for maintainers
- 🔒 **[Security Policy](/.github/SECURITY.md)** - Report vulnerabilities

---

## ⚠️ Legal & Ethical Use

**CRITICAL:** This tool is for **authorized security testing only**.

### ✅ Authorized Uses
- Testing your own accounts and credentials
- Authorized penetration testing with written permission
- Security research with appropriate authorization
- Incident response and breach validation
- Educational purposes in controlled environments

### ❌ Prohibited Uses
- Unauthorized access to systems or data
- Testing credentials without explicit permission
- Harassment or stalking
- Any illegal activities
- Violation of terms of service

**Always comply with:**
- Applicable laws and regulations
- Terms of service for all APIs used
- Organizational policies
- Ethical hacking guidelines

---

## 🛠️ Advanced Configuration

### Environment Variables

```bash
# Optional - for Have I Been Pwned
HIBP_API_KEY=your_hibp_key_here

# Optional - for LeakCheck
LEAKCHECK_API_KEY=your_leakcheck_key_here

# Optional - Custom rate limits
RATE_LIMIT_PROXYNOVA=1.0
RATE_LIMIT_HIBP=1.5
```

### Command-Line Options

```bash
breach-checker check INPUT_FILE [OPTIONS]

Options:
  -o, --output PREFIX       Output file prefix
  --proxynova              Use ProxyNova COMB (default)
  --hibp                   Use Have I Been Pwned
  --leakcheck              Use LeakCheck
  --hibp-key KEY           HIBP API key
  --leakcheck-key KEY      LeakCheck API key
  -v, --verbose            Verbose logging
  -h, --help               Show help message
```

---

## 🔮 Roadmap

### Upcoming Features
- [ ] Additional API integrations (DeHashed, Snusbase, Intelligence X)
- [ ] Web UI dashboard
- [ ] Real-time monitoring capabilities
- [ ] Email notifications for new breaches
- [ ] Webhook integrations
- [ ] Docker containerization
- [ ] Cloud deployment options

### Community Requests
We welcome feature requests! [Open an issue](https://github.com/Fused-Gaming/breach-around/issues) to suggest improvements.

---

## 🤝 Contributing

We welcome contributions! See [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines.

### Quick Contribution Guide
1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

---

## 📞 Support

### Getting Help
- 📖 **Documentation**: Check the docs in this repository
- 🐛 **Bug Reports**: [Open an issue](https://github.com/Fused-Gaming/breach-around/issues)
- 💡 **Feature Requests**: [Start a discussion](https://github.com/Fused-Gaming/breach-around/discussions)
- 🔒 **Security Issues**: See [SECURITY.md](/.github/SECURITY.md)

### Community
- **Maintainer**: Fused Gaming / VLN Lab
- **Repository**: [github.com/Fused-Gaming/breach-around](https://github.com/Fused-Gaming/breach-around)

---

## 📜 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

## 🎓 Educational Resources

- [OWASP - Credential Stuffing](https://owasp.org/www-community/attacks/Credential_stuffing)
- [Have I Been Pwned - About](https://haveibeenpwned.com/About)
- [Troy Hunt's Blog - Breach Analysis](https://www.troyhunt.com/)
- [NIST - Digital Identity Guidelines](https://pages.nist.gov/800-63-3/)

---

## 🏆 Acknowledgments

- Troy Hunt for Have I Been Pwned
- The security research community
- All contributors and supporters

---

**Version:** 0.1.0
**Last Updated:** 2026-01-09
**Maintained by:** Fused Gaming / VLN Lab

---

<div align="center">

**⚡ Built for Security Professionals | 🛡️ Designed for Protection | 🎯 Focused on Results**

</div>
