# Environment Setup Guide

This guide will help you set up the environment for the Binance Futures Testnet Trading Bot.

## Prerequisites

- Python 3.8 or higher
- Windows OS (for batch/PowerShell scripts)

## Step 1: Virtual Environment Already Created ✓

A Python virtual environment has been created in the `venv` folder with all dependencies installed:
- FastAPI
- Uvicorn
- Requests
- Pydantic

## Step 2: Configure API Keys

Get your free Binance Testnet API keys from: **https://testnet.binance.vision/**

### Option A: Using PowerShell (Recommended for Windows 10+)

Run the setup script:
```bash
.\setup_env.ps1
```

Then restart your terminal to apply the changes.

### Option B: Using Batch File

Run the setup script:
```bash
setup_env.bat
```

Then restart your command prompt to apply the changes.

### Option C: Manual Configuration

Edit `backend/app/config.py` and replace:
```python
BINANCE_API_KEY = 'your_testnet_api_key_here'
BINANCE_SECRET_KEY = 'your_testnet_secret_key_here'
```

Or set environment variables:
```powershell
[Environment]::SetEnvironmentVariable("BINANCE_TESTNET_API_KEY", "your_key", [EnvironmentVariableTarget]::User)
[Environment]::SetEnvironmentVariable("BINANCE_TESTNET_SECRET_KEY", "your_secret", [EnvironmentVariableTarget]::User)
```

## Step 3: Start the Backend Server

### Option A: Using PowerShell

```bash
.\run_server.ps1
```

### Option B: Using Batch File

```bash
.\run_server.bat
```

### Option C: Manual Start

```bash
venv\Scripts\activate
cd app
python main.py
```

The server will start on: **http://localhost:8000**

## Step 4: Open Frontend

Open `frontend/index.html` in your web browser to use the trading bot UI.

## Step 5: Test the Application

1. Fill in the order form:
   - Symbol: BTCUSDT
   - Side: BUY
   - Order Type: MARKET
   - Quantity: 0.01

2. Click "Place Order"

3. Check the response in the output section

## Folder Structure

```
Trading Bot/
├── backend/
│   ├── venv/                 # Virtual environment (do not modify)
│   ├── app/
│   │   ├── main.py          # FastAPI app
│   │   ├── client.py        # Binance API client
│   │   ├── orders.py        # Order logic
│   │   ├── validators.py    # Input validation
│   │   ├── logging_config.py # Logging
│   │   └── config.py        # Configuration
│   ├── requirements.txt      # Dependencies
│   ├── run_server.ps1       # PowerShell startup script
│   ├── run_server.bat       # Batch startup script
│   ├── setup_env.ps1        # API key setup (PowerShell)
│   └── setup_env.bat        # API key setup (Batch)
├── frontend/
│   ├── index.html           # Main UI
│   ├── style.css            # Styling
│   └── script.js            # JavaScript
├── logs/
│   └── trading.log          # Application logs
└── README.md                # Main documentation
```

## Troubleshooting

### "ModuleNotFoundError: No module named 'fastapi'"
- Make sure you're using the virtual environment
- Run: `venv\Scripts\pip install -r requirements.txt`

### "Connection refused" error
- Make sure the backend server is running
- Check that http://localhost:8000 is accessible

### "Invalid API key" error
- Verify your Testnet API keys from https://testnet.binance.vision/
- Check that environment variables are set correctly
- Restart your terminal after setting environment variables

### CORS errors in browser console
- This is expected for local development
- The frontend makes requests directly to `http://localhost:8000`

## Useful Commands

```bash
# Activate virtual environment
venv\Scripts\activate

# Deactivate virtual environment
deactivate

# Install additional packages
venv\Scripts\pip install package_name

# List installed packages
venv\Scripts\pip list

# View logs
type ..\logs\trading.log
```

## API Endpoints

### Health Check
```
GET http://localhost:8000/
```

### Place Order
```
POST http://localhost:8000/order
Content-Type: application/json

{
  "symbol": "BTCUSDT",
  "side": "BUY",
  "type": "MARKET",
  "quantity": 0.01,
  "price": null
}
```

## Next Steps

1. ✓ Environment created
2. ✓ Dependencies installed
3. Configure your API keys (Step 2)
4. Start the server (Step 3)
5. Open the frontend (Step 4)
6. Test with a sample order (Step 5)

Happy trading! 🚀