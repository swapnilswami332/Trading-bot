# Quick Start Guide

Get your Binance Testnet Trading Bot up and running in 3 simple steps!

## 1. Configure API Keys (First Time Only)

**PowerShell:**
```bash
cd backend
.\setup_env.ps1
```

**Batch:**
```bash
cd backend
setup_env.bat
```

Or manually edit `backend/app/config.py` with your keys.

**Get free testnet keys:** https://testnet.binance.vision/

---

## 2. Start Backend Server

**PowerShell:**
```bash
cd backend
.\run_server.ps1
```

**Batch:**
```bash
cd backend
run_server.bat
```

**Manual:**
```bash
cd backend
venv\Scripts\activate
cd app
python main.py
```

You should see:
```
INFO:     Application startup complete [uvicorn]
INFO:     Uvicorn running on http://127.0.0.1:8000
```

---

## 3. Open Frontend

Open `frontend/index.html` in your web browser.

Or serve it locally:
```bash
cd frontend
python -m http.server 3000
```

Then visit: `http://localhost:3000`

---

## Test It Out

1. Enter symbol: `BTCUSDT`
2. Select side: `BUY`
3. Select type: `MARKET`
4. Enter quantity: `0.01`
5. Click "Place Order"

---

## API Endpoint

```
POST http://localhost:8000/order

{
  "symbol": "BTCUSDT",
  "side": "BUY",
  "type": "MARKET",
  "quantity": 0.01,
  "price": null
}
```

---

## Stop Server

Press `Ctrl + C` in the terminal

---

## Support

- Backend logs: `logs/trading.log`
- Main docs: `README.md`
- Setup guide: `ENVIRONMENT_SETUP.md`
- API keys: https://testnet.binance.vision/

---

✨ **You're ready to go!** Happy trading! 🚀