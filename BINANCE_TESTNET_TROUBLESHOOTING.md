# Binance Testnet API Troubleshooting Guide

## Issue: `Invalid API-key, IP, or permissions for action`

This error means ONE of these 3 things:
1. ❌ API Key itself is invalid/wrong
2. ❌ Missing Testnet Trading Permissions
3. ❌ Your IP is blocked by IP Whitelist

---

## STEP 1: Check If API Key is Valid
### On Binance Testnet Website:

1. Go to: **https://testnet.binancefuture.com**
2. Login with your account
3. Click on **Account** (top right profile icon)
4. Select **API Management**
5. Look for your API Key (should start with letters)

**If you see the key listed** → ✅ Key exists
**If you don't see it** → ❌ Key is invalid/doesn't exist - CREATE A NEW ONE

---

## STEP 2: Check Testnet Permissions
### In API Management page, look for these settings:

```
Your API Keys
├── API Key: [your_key_here]
└── Settings for this API Key:
    ├── ✅ Enable Margin Trading         (Must be CHECKED)
    ├── ✅ Enable Futures Trading        (Must be CHECKED)
    └── ✅ Enable Spot Trading           (Should be checked)
```

### If NOT checked:

1. Click **Edit** next to your API key
2. **CHECK** these boxes:
   - ✅ `Enable Margin Trading`
   - ✅ `Enable Futures Trading`
3. Click **Save**
4. **Wait 5-10 seconds** for settings to apply

---

## STEP 3: Check IP Restrictions
### Still in API Management page, look for:

```
IP Whitelist Status
├── Type 1: "No restriction" 
│        └── ✅ Your app can connect from ANY IP
│
└── Type 2: "Restrict to IP..." 
         └── ❌ Your app can ONLY connect from listed IPs
```

### How to check what's set:

1. In API Management, check under **Restrictions** or **IP Access Restriction**
2. If it says **"Restrict to IP..."** → You have IP whitelist enabled
3. Below it should show: `[IP addresses listed]`

### To find your CURRENT IP:

**Option A: Use Python Script**
```python
# Run this in your terminal:
python -c "import requests; print('Your current IP:', requests.get('https://api.ipify.org').text)"
```

**Option B: Use PowerShell**
```powershell
(Invoke-RestMethod -Uri 'https://api.ipify.org').trim()
```

**Option C: Online**
- Go to: https://whatismyipaddress.com
- Copy your IPv4 address

### Compare IPs:

```
If IP Whitelist shows:    Your Current IP is:
├── 192.168.1.100         └── 203.45.67.89
├── 192.168.1.101            
└── 192.168.1.102         ❌ DOES NOT MATCH!
```

---

## STEP 4: Fix IP Restriction Issues

### Option A: REMOVE ALL IP RESTRICTIONS (EASIEST)
1. In API Management → **Restrictions**
2. Click **Edit** → Select **"No Restriction"**
3. Click **Save**
4. ✅ Your app can now connect from any IP

### Option B: ADD YOUR CURRENT IP TO WHITELIST
1. Find your current IP (see Step 3 above)
2. In API Management → **Restrictions**
3. Click **Edit** → Select **"Restrict to IP..."**
4. Enter your IP in the field: `203.45.67.89` (example)
5. Click **Add** or **+**
6. Click **Save**

**IMPORTANT**: If your ISP changes your IP, the connection will fail again!

---

## STEP 5: Test Your API Keys

### Quick Test with Python Script:

Create file: `test_api_keys.py`

```python
import requests
import hmac
import hashlib
import json
from datetime import datetime

# YOUR API KEYS - UPDATE THESE
API_KEY = "your_api_key_here"
SECRET_KEY = "your_secret_key_here"

def test_binance_connection():
    base_url = "https://testnet.binancefuture.com"
    endpoint = "/fapi/v1/account"
    
    # Create signature
    timestamp = int(datetime.now().timestamp() * 1000)
    query_string = f"timestamp={timestamp}"
    
    signature = hmac.new(
        SECRET_KEY.encode(),
        query_string.encode(),
        hashlib.sha256
    ).hexdigest()
    
    # Send request
    headers = {"X-MBX-APIKEY": API_KEY}
    params = {"timestamp": timestamp, "signature": signature}
    
    print("=" * 60)
    print("TESTING BINANCE TESTNET CONNECTION")
    print("=" * 60)
    print(f"API Key (first 10 chars): {API_KEY[:10]}...")
    print(f"Secret Key (first 10 chars): {SECRET_KEY[:10]}...")
    print(f"Timestamp: {timestamp}")
    print(f"Signature: {signature}")
    print()
    
    try:
        response = requests.get(
            f"{base_url}{endpoint}",
            headers=headers,
            params=params,
            timeout=5
        )
        
        print(f"Status Code: {response.status_code}")
        print(f"Response: {json.dumps(response.json(), indent=2)}")
        
        if response.status_code == 200:
            print("\n✅ SUCCESS! API Keys are valid!")
        else:
            print(f"\n❌ ERROR: {response.json()}")
            
    except Exception as e:
        print(f"❌ Connection failed: {e}")

if __name__ == "__main__":
    test_binance_connection()
```

**How to run it:**
```powershell
cd "c:\swapnil program1\Trading Bot\backend\app"
python test_api_keys.py
```

---

## STEP 6: Verify in config.py

Once everything is set up, verify your config.py has the correct keys:

```python
# config.py
BINANCE_API_KEY = 'xdxJeBoOKW7qGDUybF2lLR6nh9Rf0MUQS6a5I3hGVg1Uuwy2kVdm5iRWgOgFVdlD'
BINANCE_SECRET_KEY = 'cukOrBXnqb2MX0tkkN8TaNzGeSKQrlnPEX0WHSLpymJ7Sv38tOvfj0FNVus2gkhe'
```

---

## QUICK CHECKLIST

```
✓ API Key exists on Binance Testnet
  └─ Verify at: https://testnet.binancefuture.com → Account → API Management

✓ Permissions are enabled:
  └─ ✅ Enable Margin Trading
  └─ ✅ Enable Futures Trading

✓ IP Restriction status:
  └─ Option 1: "No Restriction" (Recommended for testing)
  └─ Option 2: Your current IP is in the whitelist

✓ Test API Keys with test_api_keys.py script above
  └─ Should show ✅ SUCCESS if all is correct

✓ Keys are correctly copied to config.py (no extra spaces)
  └─ Verify exact match character-by-character
```

---

## Most Common Causes (in order):

1. **50%**: IP Whitelist blocking your IP
   - **Fix**: Remove restriction or add your IP

2. **30%**: Missing Futures Trading permission
   - **Fix**: Enable "Enable Futures Trading" checkbox

3. **15%**: Wrong/invalid API keys
   - **Fix**: Delete and create new API keys

4. **5%**: Extra spaces in key (invisible character)
   - **Fix**: Copy-paste key again from Binance carefully
