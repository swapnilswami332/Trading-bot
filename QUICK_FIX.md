# 🚨 Quick Fix Checklist

## Root Cause
Your Binance API credentials are **invalid or expired**:
```
Error: {"code":-2015,"msg":"Invalid API-key, IP, or permissions for action"}
```

## 🎯 What You Need to Do

### Step 1: Get New Credentials ⏱️ (5 minutes)

1. Go to: https://testnet.binancefuture.com
2. Login/Sign up if needed
3. Go to **Account → API Management** (or **API Keys**)
4. Click **"Create API"**
5. Label it "Trading Bot"
6. Copy the **API Key** and **Secret Key**

```
Example:
API Key:     xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
Secret Key:  xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
```

---

### Step 2: Test Your Credentials ⏱️ (1 minute)

Update [setup_and_test.ps1](setup_and_test.ps1) with your new credentials:

```powershell
$API_KEY = "YOUR_API_KEY_HERE"
$SECRET_KEY = "YOUR_SECRET_KEY_HERE"
```

Then run it:

```powershell
cd "c:\swapnil program1\Trading Bot\backend"
.\setup_and_test.ps1
```

---

### Step 3: Check the Output

#### ✅ If you see:
```
✅ SUCCESS! Account data received:
  Balance USDT: 10000.00
  ✅ Your credentials are VALID and working!
```

**Great!** Your credentials work. Go to Step 4.

#### ❌ If you still see 401 error:
```
❌ 401 Unauthorized - Your credentials are INVALID or expired
```

Then:
- ❌ Delete that API key and create a **NEW** one
- ❌ Make sure it's a **FUTURES** API key (not Spot)
- ❌ Add IP whitelist (if needed): `127.0.0.1`

---

### Step 4: Start the Server

If credentials test passed:

```powershell
cd "c:\swapnil program1\Trading Bot\backend\app"
python main.py
```

---

### Step 5: Test Your Order

Make a POST request to http://localhost:8000/order:

```json
{
  "symbol": "BTCUSDT",
  "side": "BUY",
  "type": "MARKET",
  "quantity": 0.01,
  "price": 30
}
```

You should get a **200 response** with an order ID.

---

## 📋 Files Created to Help You

1. **test_credentials.py** - Validates your credentials are working
2. **setup_and_test.ps1** - Automated setup and testing script
3. **CREDENTIAL_FIX_GUIDE.md** - Detailed troubleshooting guide

---

## 🆘 Still Not Working?

Share these logs:
1. Output from `python test_credentials.py`
2. Check if you're using **Futures** API key (not Spot)
3. Check IP whitelist in Binance settings

---

## ⚠️ Important Security Notes

- ❌ **DON'T** commit credentials to git
- ❌ **DON'T** expose API keys in logs
- ✅ Use environment variables for production
- ✅ Use IP whitelist for security
- ✅ Rotate keys regularly
