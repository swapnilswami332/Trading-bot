# Binance Futures Testnet Trading Bot

A full-stack web application for placing orders on Binance Futures Testnet. This application provides a simple web interface to interact with the Binance Futures Testnet API for placing MARKET and LIMIT orders.

## Features

- **Backend**: Python FastAPI application with Binance Futures Testnet integration using direct REST API calls
- **Frontend**: Simple web interface with form validation and real-time feedback
- **Order Types**: Support for MARKET and LIMIT orders
- **Validation**: Comprehensive input validation on both frontend and backend
- **Logging**: Detailed logging of all API requests and responses
- **Error Handling**: Proper error handling for API failures and invalid inputs

## Project Structure

```
trading_bot/
  backend/
    app/
      main.py          # FastAPI application
      client.py        # Binance API client wrapper
      orders.py        # Order service logic
      validators.py    # Input validation functions
      logging_config.py # Logging configuration
      config.py        # Configuration settings
    requirements.txt   # Python dependencies
  frontend/
    index.html         # Main HTML page
    style.css          # CSS styling
    script.js          # JavaScript functionality
  logs/
    trading.log        # Application logs
  README.md            # This file
```

## Prerequisites

- Python 3.8+
- Binance Futures Testnet API keys (get them from [Binance Testnet](https://testnet.binance.vision/))

## Setup

1. **Clone or download the project files**

2. **Set up environment variables for API keys:**
   ```bash
   export BINANCE_TESTNET_API_KEY='your_api_key_here'
   export BINANCE_TESTNET_SECRET_KEY='your_secret_key_here'
   ```
   Or edit `backend/app/config.py` directly with your keys.

3. **Install backend dependencies:**
   ```bash
   cd backend
   pip install -r requirements.txt
   ```

## Running the Application

### Backend

1. Navigate to the backend directory:
   ```bash
   cd backend/app
   ```

2. Run the FastAPI server:
   ```bash
   python main.py
   ```

   The API will be available at `http://localhost:8000`

### Frontend

1. Open `frontend/index.html` in your web browser

   Or serve it with a local server:
   ```bash
   # Using Python
   cd frontend
   python -m http.server 3000
   ```

   Then open `http://localhost:3000` in your browser

## API Usage

### Place Order

**Endpoint:** `POST /order`

**Request Body:**
```json
{
  "symbol": "BTCUSDT",
  "side": "BUY",
  "type": "MARKET",
  "quantity": 0.01,
  "price": 50000.00  // Required only for LIMIT orders
}
```

**Response:**
```json
{
  "orderId": "123456789",
  "status": "FILLED",
  "executedQty": "0.01",
  "avgPrice": "50000.00",
  "fullResponse": { ... }
}
```

## Validation Rules

- **Symbol**: Must be uppercase string (e.g., BTCUSDT)
- **Side**: Must be "BUY" or "SELL"
- **Type**: Must be "MARKET" or "LIMIT"
- **Quantity**: Must be positive number
- **Price**: Required for LIMIT orders, must be positive number

## Logging

All API requests, responses, and errors are logged to `logs/trading.log` with timestamps and log levels.

## Error Handling

The application handles:
- Invalid input validation
- Binance API errors
- Network failures
- Missing required parameters

Error responses include clear JSON messages.

## Assumptions

- User has valid Binance Futures Testnet API keys
- User understands trading risks (this is for educational purposes only)
- Testnet funds are available for placing orders
- CORS is handled appropriately for frontend-backend communication

## Security Notes

- Never commit API keys to version control
- Use environment variables for sensitive data
- This is a testnet implementation - adapt for mainnet with caution
- Implement additional security measures for production use

## Bonus Features

- **Improved UI**: Enhanced user experience with better validation messages, loading states, and responsive design
- Form validation with real-time feedback
- Dynamic price field visibility
- Clear success/error messaging with formatted JSON responses