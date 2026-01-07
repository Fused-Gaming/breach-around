# Breach Around - Breach Checker and OSINT toolkit

A comprehensive, multi-service breach database checker with support for multiple APIs and organized credential validation.

## 🚀 README

### Windows
```bash
cd breach-checker
run_checker.bat input_data\list.csv
```

### Linux/Mac
```bash
cd /path/to/breach-checker
./run_checker.sh input_data/list.csv
```

## 📁 Directory Structure

```
breach-checker/
├── breach_apis/              # Breach database API integrations
│   ├── proxynova_comb.py    # ProxyNova COMB API (FREE)
│   ├── haveibeenpwned.py    # Have I Been Pwned API
│   └── leakcheck.py         # LeakCheck.io API
├── utils/                    # Utility modules
│   ├── credential_parser.py # CSV parsing and credential handling
│   └── api_helpers.py       # Rate limiting, retries, request helpers
├── input_data/              # Input CSV files
├── results/                 # Output results
├── logs/                    # Application logs
├── unified_breach_checker.py # Main unified checker script
├── run_checker.bat          # Windows launcher
├── run_checker.sh           # Linux/Mac launcher
└── requirements.txt         # Python dependencies
```

## 🔧 Installation

### 1. Create Virtual Environment (Already Done)
The virtual environment is already set up in the breach-checker directory.

### 2. Activate Virtual Environment

**Windows:**
```bash
Scripts\activate
```

**Linux/Mac:**
```bash
source bin/activate
```

### 3. Verify Installation
```bash
pip list | grep -E "(requests|selenium|python-dotenv)"
```

## 🌐 Supported Services

### 1. ProxyNova COMB (FREE - Default)
- **API:** Public, no key required
- **Database:** Compilation of Many Breaches (COMB)
- **Features:** Email and password breach lookup
- **Rate Limit:** ~1 request/second
- **Cost:** Free

### 2. Have I Been Pwned (HIBP)
- **API:** Free password check, paid account lookup
- **Features:**
  - Password breach count (FREE via k-anonymity)
  - Account breach history (PAID - requires API key)
- **Rate Limit:** 1 request per 1.5 seconds
- **Cost:** Password check free, account lookup $3.50/month
- **Get API Key:** https://haveibeenpwned.com/API/Key

### 3. LeakCheck.io
- **API:** Paid service with free tier
- **Features:** Comprehensive breach database
- **Rate Limit:** Based on subscription
- **Cost:** Free tier available, paid plans start at $2
- **Get API Key:** https://leakcheck.io/api

## 📝 Configuration

### Environment Variables

Copy `.env.example` to `.env` and configure:

```bash
cp .env.example .env
```

Edit `.env`:
```env
# Optional - for Have I Been Pwned account lookup
HIBP_API_KEY=your_hibp_key_here

# Optional - for LeakCheck service
LEAKCHECK_API_KEY=your_leakcheck_key_here
```

## 🎯 Usage

### Basic Usage (ProxyNova - Free)

Check a single CSV file:
```bash
# Windows
run_checker.bat input_data\list.csv

# Linux/Mac
./run_checker.sh input_data/list.csv
```

### Advanced Usage

**With Have I Been Pwned:**
```bash
python unified_breach_checker.py input_data/list.csv --hibp --hibp-key YOUR_KEY
```

**With LeakCheck:**
```bash
python unified_breach_checker.py input_data/list.csv --leakcheck --leakcheck-key YOUR_KEY
```

**All Services:**
```bash
python unified_breach_checker.py input_data/list.csv \
  --proxynova \
  --hibp --hibp-key YOUR_HIBP_KEY \
  --leakcheck --leakcheck-key YOUR_LEAKCHECK_KEY
```

**Custom Output:**
```bash
python unified_breach_checker.py input_data/list.csv -o results/custom_check
```

### Command-Line Options

```
positional arguments:
  input_file            Input CSV file with credentials

optional arguments:
  -h, --help            Show help message
  -o, --output PREFIX   Output file prefix (default: results/breach_check)
  --proxynova           Use ProxyNova COMB (default: True)
  --hibp                Use Have I Been Pwned
  --leakcheck           Use LeakCheck
  --hibp-key KEY        HIBP API key
  --leakcheck-key KEY   LeakCheck API key
```

## 📋 Input Format

Your CSV files should contain credentials in one of these formats:

### Format 1: With Metadata
```csv
email,pass,balance,value,
user@example.com:Password123 | Balance = 50 | CertificateValue = 10
another@email.com:SecurePass | Balance = 0 | CertificateValue = 0
```

### Format 2: Simple
```csv
email:password
user@example.com:Password123
another@email.com:SecurePass
```

### Format 3: Comma-Separated
```csv
email,password
user@example.com,Password123
another@email.com,SecurePass
```

The parser automatically detects and handles all formats.

## 📊 Output Files

### 1. CSV Summary (`*_YYYYMMDD_HHMMSS.csv`)
```csv
email,password,overall_status,total_breaches,password_compromised,breached_services,clean_services
user@test.com,Pass123,breached,15,YES,"ProxyNova, HIBP",""
safe@test.com,Secure456,clean,0,NO,"","ProxyNova, HIBP"
```

### 2. JSON Results (`*_YYYYMMDD_HHMMSS.json`)
Complete structured data with all service responses:
```json
[
  {
    "email": "user@example.com",
    "password": "Password123",
    "timestamp": "2024-01-07T10:30:00",
    "services": {
      "ProxyNova": { ... },
      "HIBP": { ... }
    },
    "summary": {
      "overall_status": "breached",
      "total_breaches": 15,
      "password_compromised": true
    }
  }
]
```

### 3. Detailed Log (`*_YYYYMMDD_HHMMSS_detailed.txt`)
Human-readable report with full details for each credential.

## 🔍 Example Workflow

### Scenario: Check List of Emails for Breaches

1. **Prepare Input Data**
   ```bash
   # Your CSV file is already in input_data/
   ls input_data/
   ```

2. **Run Free Check (ProxyNova)**
   ```bash
   run_checker.bat input_data\list.csv
   ```

3. **Review Results**
   ```bash
   # Check CSV summary
   type results\breach_check_20240107_103000.csv

   # Check detailed log
   type results\breach_check_20240107_103000_detailed.txt
   ```

4. **Analyze Summary**
   - Total checked: X
   - Breached: Y (Z%)
   - Password compromised: W (V%)

## 🎨 Features

### ✅ Multi-Service Support
- Check against multiple breach databases simultaneously
- Aggregate results from all services
- Fallback if one service fails

### ✅ Intelligent Rate Limiting
- Automatic rate limiting per service
- Exponential backoff on failures
- Concurrent checking where possible

### ✅ Comprehensive Reporting
- CSV for spreadsheet analysis
- JSON for programmatic use
- Detailed logs for investigation

### ✅ Password Analysis
- Detect if specific password was breached
- Find all passwords associated with email
- Check password strength via HIBP

### ✅ Robust Error Handling
- Retry failed requests
- Continue on errors
- Log all issues

## 🔐 Security Best Practices

### Password Checking (HIBP)
HIBP uses k-anonymity for password checks:
1. Hash password with SHA-1
2. Send first 5 characters to API
3. API returns all matching hashes
4. Compare locally - password never sent!

### API Keys
- Store in `.env` file (gitignored)
- Never commit API keys to repository
- Use environment variables in production

### Rate Limiting
- Respect API rate limits
- Default delays prevent blocking
- Automatic backoff on errors

## 📈 Performance

### Estimated Check Times

| Service | Rate Limit | 100 Credentials | 1000 Credentials |
|---------|-----------|----------------|------------------|
| ProxyNova | 1/sec | ~2 minutes | ~17 minutes |
| HIBP | 1/1.5sec | ~3 minutes | ~25 minutes |
| LeakCheck | Varies | ~2-5 minutes | ~20-50 minutes |

### Optimization Tips

1. **Use ProxyNova Only for Fast Checks**
   ```bash
   run_checker.bat input_data\list.csv  # Default, fastest
   ```

2. **Run Services in Sequence (Recommended)**
   - The unified checker runs services sequentially per credential
   - This ensures accurate per-credential results

3. **Batch Process Multiple Files**
   ```bash
   for file in input_data/*.csv; do
       run_checker.bat "$file"
   done
   ```

## 🛠️ Troubleshooting

### Issue: API Rate Limited
**Solution:** Increase delay in code or wait before retrying

### Issue: Invalid JSON Response
**Solution:** Check API status, may be down temporarily

### Issue: Connection Timeout
**Solution:** Check internet connection, increase timeout in code

### Issue: No Results Found
**Solution:** Verify CSV format, check email addresses are valid

### Issue: Module Not Found
**Solution:**
```bash
# Reactivate virtual environment
Scripts\activate  # Windows
source bin/activate  # Linux/Mac

# Reinstall requirements
pip install -r requirements.txt
```

## 🔮 Future Enhancements

### Planned Features
- [ ] DeHashed API integration
- [ ] Snusbase API integration
- [ ] Intelligence X API integration
- [ ] Parallel multi-service checking
- [ ] Web UI dashboard
- [ ] Real-time monitoring
- [ ] Email notifications
- [ ] Webhook integrations
- [ ] Export to Excel with charts

### Additional Services to Add
- **DeHashed** - Comprehensive breach database
- **Snusbase** - Deep web breach data
- **Intelligence X** - Dark web monitoring
- **SpyCloud** - Enterprise breach prevention

## 📚 API Documentation

### ProxyNova COMB
- **Docs:** Limited public documentation
- **Endpoint:** `https://api.proxynova.com/comb`
- **Parameters:** `query`, `start`, `limit`

### Have I Been Pwned
- **Docs:** https://haveibeenpwned.com/API/v3
- **Endpoints:**
  - `/breachedaccount/{account}` (paid)
  - Pwned Passwords API (free)

### LeakCheck
- **Docs:** https://leakcheck.io/api-documentation
- **Endpoint:** `/api/public`
- **Parameters:** `key`, `check`, `type`

## 🤝 Contributing

To add a new breach service:

1. Create new file in `breach_apis/`:
   ```python
   # breach_apis/newservice.py
   class NewServiceChecker:
       def check_credential(self, credential):
           # Implementation
           pass
   ```

2. Add to `unified_breach_checker.py`:
   ```python
   from breach_apis.newservice import NewServiceChecker
   # Add initialization and usage
   ```

3. Update README with service details

## 📄 License

This tool is for authorized security testing only. See LICENSE for details.

## ⚠️ Legal Notice

**IMPORTANT:** Only use this tool with proper authorization:

✅ **Authorized Uses:**
- Testing your own accounts
- Authorized penetration testing
- Security research with permission
- Data breach response and validation

Always comply with applicable laws and terms of service. This repository is for security research only.

## 📞 Support

For issues or questions:
1. Check existing CSV files in `input_data/`
2. Review logs in `logs/breach_checker.log`
3. Verify API keys in `.env`
4. Check service status pages

## 🎓 Educational Resources

- [OWASP - Credential Stuffing](https://owasp.org/www-community/attacks/Credential_stuffing)
- [Have I Been Pwned - About](https://haveibeenpwned.com/About)
- [Troy Hunt's Blog - Breach Analysis](https://www.troyhunt.com/)
- [NIST - Digital Identity Guidelines](https://pages.nist.gov/800-63-3/)

---

**Version:** 

`1.0.0`

**Last Updated:**

2026-01-07

**Maintainer:** 

Fused Gaming / VLN Lab
