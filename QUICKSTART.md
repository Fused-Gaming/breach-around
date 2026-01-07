# Breach Around - Breach Checker and OSINT toolkit: Quick Start Guide

## ⚡ 5-Minute Setup

### Step 1: Activate Environment (10 seconds)
```bash
cd breach-around
Scripts\activate
```

### Step 2: Verify Setup (30 seconds)
```bash
python test_setup.py
```

Expected output:
```
✓ PASS - Imports
✓ PASS - Directories
✓ PASS - Credential Parser
✓ PASS - API Modules
```

### Step 3: Run Your First Check (2-5 minutes)
```bash
run_checker.bat input_data\list.csv
```

### Step 4: View Results
```bash
# Open CSV summary
type results\breach_check_*.csv

# View detailed log
type results\breach_check_*_detailed.txt
```

## 🎯 Common Use Cases

### Check Single File
```bash
run_checker.bat input_data\nike.csv
```

### Check All Files
```bash
for %f in (input_data\*.csv) do run_checker.bat %f
```

### Custom Output Location
```bash
python unified_breach_checker.py input_data/list.csv -o results/custom_check
```

## 📊 What You'll Get

### 1. CSV Summary (Excel-ready)
| Email | Password | Status | Breaches | Password Compromised |
|-------|----------|--------|----------|---------------------|
| user@test.com | Pass123 | breached | 15 | YES |
| safe@test.com | Secure! | clean | 0 | NO |

### 2. JSON Results (API-ready)
Complete structured data for programmatic use

### 3. Detailed Log (Investigation-ready)
Full breach details, all passwords, related entries

## 🚀 Advanced Usage (Optional)

### With Have I Been Pwned
```bash
# Add to .env file:
HIBP_API_KEY=your_key_here

# Run with HIBP
python unified_breach_checker.py input_data/list.csv --hibp
```

### With Multiple Services
```bash
python unified_breach_checker.py input_data/list.csv \
  --proxynova \
  --hibp --hibp-key YOUR_KEY \
  --leakcheck --leakcheck-key YOUR_KEY
```

## ❓ Troubleshooting

### Issue: "Module not found"
**Solution:**
```bash
Scripts\activate
pip install -r requirements.txt
```

### Issue: "No CSV files found"
**Solution:** CSV files should be in `input_data/` directory

### Issue: Script fails
**Solution:**
```bash
python test_setup.py  # Diagnose issue
```

## 📚 Next Steps

1. ✅ Read full documentation: [BREACH_CHECKER_README.md](BREACH_CHECKER_README.md)
2. ✅ Check project structure: [PROJECT_INDEX.md](PROJECT_INDEX.md)
3. ✅ Review results and analyze data
4. ✅ Consider setting up API keys for additional services

## 🎉 You're Ready!

Your breach checker is now set up and ready to use. Start checking your credentials against breach databases!

---

**Questions?** Check [BREACH_CHECKER_README.md](BREACH_CHECKER_README.md) for detailed documentation.
