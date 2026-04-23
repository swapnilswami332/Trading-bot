# 🔧 Fixing 401 Unauthorized Error - Setup Guide

## Problem
You're getting: `{"code":-2015,"msg":"Invalid API-key, IP, or permissions for action"}`

This means your **API credentials are invalid, expired, or not properly configured**.

---

## ✅ Step 1: Test Current Credentials

Run the diagnostic script to identify the exact issue:

```powershell
cd "c:\swapnil program1\Trading Bot\backend\app"
python test_credentials.py
```

This will tell you if:
- ✓ Credentials are valid
- ✗ Credentials are invalid/expired
- ✗ IP is whitelisted
- ✗ System clock is out of sync

---

## 🔑 Step 2: Generate New API Credentials (Binance Testnet)

### Go to Binance Testnet:
1. **Visit**: https://testnet.binancefuture.com
2. **Sign in** (or create a testnet account if you don't have one)
3. **Go to**: Account → API Management (or Futures Account → API Keys)

### Create New API Key:
1. Click **"Create API"**
2. **Label**: e.g., "Trading Bot"
3. Select **"Futures" API Type**
4. Click **Generate** (or next/create)
5. You'll see your **API Key** and **Secret Key**

### Important Settings:
- **IP Whitelist**: If you're testing locally, add `127.0.0.1` 
- For production, use your server's public IP
- Or leave blank if you trust the network (not recommended for production)

### Copy & Save:
```
API Key:     BINANCE_API_KE
Secret Key:  BINANCE_SECRET_KEY
```

⚠️ **NEVER commit these to git! Store them in environment variables instead.**

---

## 🔐 Step 3: Store Credentials Securely (Two Options)

### Option A: Environment Variables (RECOMMENDED ✅)

**On Windows PowerShell:**

```powershell
$env:BINANCE_TESTNET_API_KEY = "your_api_key_here"
$env:BINANCE_TESTNET_SECRET_KEY = "your_secret_key_here"

# Verify they're set
echo $env:BINANCE_TESTNET_API_KEY
```

**Permanently (Edit your PowerShell Profile):**

```powershell
# Open your profile
notepad $PROFILE

# Add these lines:
$env:BINANCE_TESTNET_API_KEY = "your_api_key_here"
$env:BINANCE_TESTNET_SECRET_KEY = "your_secret_key_here"

# Save and reload:
. $PROFILE
```

### Option B: .env File (For Development)

Create a file: `backend/app/.env`

```env
BINANCE_TESTNET_API_KEY=your_api_key_here
BINANCE_TESTNET_SECRET_KEY=your_secret_key_here
```

Then update `config.py`:

```python
from dotenv import load_dotenv
import os

load_dotenv()

BINANCE_API_KEY = os.getenv('BINANCE_TESTNET_API_KEY')
BINANCE_SECRET_KEY = os.getenv('BINANCE_TESTNET_SECRET_KEY')
BINANCE_BASE_URL = 'https://testnet.binancefuture.com'
```

Install python-dotenv:
```powershell
pip install python-dotenv
```

---

## 🚀 Step 4: Test Again

Run the diagnostic script:

```powershell
python test_credentials.py
```

Expected output if successful:
```
✅ SUCCESS! Account data received:
  Balance USDT: 10000.00
  Margin Level: 1234.56
  Positions: 0

✅ Your credentials are VALID and working!
```

---

## 🛠️ Step 5: Update config.py (Recommended)

Replace the hardcoded credentials with environment variables:

```python
# config.py
import os

# Load credentials from environment variables
BINANCE_API_KEY = os.getenv('BINANCE_TESTNET_API_KEY')
BINANCE_SECRET_KEY = os.getenv('BINANCE_TESTNET_SECRET_KEY')

if not BINANCE_API_KEY or not BINANCE_SECRET_KEY:
    raise ValueError("❌ ERROR: BINANCE_TESTNET_API_KEY and BINANCE_TESTNET_SECRET_KEY environment variables must be set!")

BINANCE_BASE_URL = 'https://testnet.binancefuture.com'

# Logging Configuration
LOG_FILE = '../../logs/trading.log'
LOG_LEVEL = 'DEBUG'
LOG_FORMAT = '%(asctime)s - %(levelname)s - %(message)s'
```

---

## 📋 Troubleshooting

### Still Getting 401?

1. **Check if API Key is VALID:**
   ```powershell
   python test_credentials.py
   ```

2. **Check for IP Whitelisting:**
   - Go to Binance Testnet API settings
   - Check if your IP is whitelisted
   - Find your IP: `curl ifconfig.me` (if you have curl installed)

3. **Check if API has correct permissions:**
   - Your API key should have "Futures" or "Trading" permission
   - Do NOT use "Spot" API keys for Futures

4. **Clear old credentials:**
   - Go to Binance → API Management
   - Delete old/unused API keys
   - Create a fresh one with proper permissions

5. **Restart everything:**
   ```powershell
   # Kill existing Python process
   taskkill /F /IM python.exe
   
   # Set environment variables
   $env:BINANCE_TESTNET_API_KEY = "your_key"
   $env:BINANCE_TESTNET_SECRET_KEY = "your_secret"
   
   # Restart server
   cd "c:\swapnil program1\Trading Bot\backend\app"
   python main.py
   ```

---

## ✅ Expected Flow After Fix

1. **Run test:** `python test_credentials.py` ✓ Shows account balance
2. **Start server:** `python main.py` ✓ Server running
3. **Place order:** POST to `/order` ✓ Gets 200 response
4. **Check logs:** ✓ Order placed successfully

---

## 🔗 Useful Links

- Binance Testnet: https://testnet.binancefuture.com
- Binance API Docs: https://binance-docs.github.io/apidocs/futures/en/
- API Key Management: https://testnet.binancefuture.com/en/usercenter/settings/api-management

---

**Next Step**: Run `python test_credentials.py` and share the output so I can see what the issue is!
