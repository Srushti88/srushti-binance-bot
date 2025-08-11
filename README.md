# Binance USDT-M Futures Trading Bot – CLI
A Python-based command-line tool for placing and managing orders on Binance USDT-M Futures Testnet.
Supports both basic and advanced strategies, with detailed logging and input validation.


## Features

## Basic Order Types
**Market Order** - Execute instantly at the current market price.
**Limit Order** -Execute only when the market reaches your set price.

## Advanced Order Types
**Stop-Limit** – Triggers a limit order once the stop price is reached.
**OCO (One-Cancels-the-Other)** – Two linked orders; execution of one cancels the other.
**TWAP (Time-Weighted Average Price)** – Breaks a large order into smaller timed chunks.
**Grid Trading** – Places multiple buy/sell orders across a price range.

## Prerequisites
Python 3.7+ installed
Binance Testnet Account: Register at https://testnet.binancefuture.com
API Credentials: Generate API key and secret from testnet

## Installation
1. Clone or download the project
2. Create virtual environment:
```bash
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
```
3. Install dependencies:
```bash
pip install -r requirements.txt
```

## Configuration
Create a `.env` file in the project root with your Binance testnet API credentials:
```
BINANCE_API_KEY=your_api_key_here
BINANCE_API_SECRET=your_api_secret_here
```
Alternatively, you can export them as environment variables. The bot reads credentials from `.env` via `python-dotenv`.

## Usage

### Basic CLI Commands

All order types can be executed directly using their respective Python files.

### Market Orders
```bash
python src/market_orders.py BTCUSDT BUY 0.01
python src/market_orders.py ETHUSDT SELL 0.1
```

### Limit Orders
```bash
python src/limit_orders.py BTCUSDT BUY 0.01 45000.00
python src/limit_orders.py ETHUSDT SELL 0.1 3500.00
```

### Advanced Orders

**Stop-Limit Orders:**
```bash
python src/advanced/stop_limit.py BTCUSDT BUY 0.01 44000.00 45000.00
```

**OCO Orders:**
```bash
python src/advanced/oco.py BTCUSDT SELL 0.01 46000.00 44000.00 43500.00
```

**TWAP Orders:**
```bash
python src/advanced/twap.py BTCUSDT BUY 0.1 300 0.01
# Parameters: SYMBOL SIDE TOTAL_QUANTITY TIME_DURATION_SECONDS CHUNK_SIZE
```

**Grid Orders:**
```bash
python src/advanced/grid.py BTCUSDT 44000.00 46000.00 5 0.01
# Parameters: SYMBOL PRICE_LOW PRICE_HIGH GRID_LEVELS QUANTITY_PER_LEVEL
```

## Project Structure

```
├── src/
│   ├── market_orders.py
│   ├── limit_orders.py
│   ├── advanced/
│   │   ├── stop_limit.py
│   │   ├── oco.py
│   │   ├── twap.py
│   │   └── grid.py
├── bot.log
├── requirements.txt
└── README.md                   
```

## Input Validation

The bot checks all order details before execution:

Side – Must be BUY or SELL.
Quantity – Positive numeric value only.
Price – Positive value for price-based orders.
Stop/Limit Rules – Validates correct relationship for stop-limit orders.


## Logging

All operations are logged to `bot.log` with structured JSON format. Timestamps default to Asia/Kolkata. You can override with:

```bash
export BOT_LOG_TZ="UTC"   # or any IANA timezone, e.g., Asia/Singapore
```

Example JSON line:

```json
{
  "timestamp": "2024-01-15T10:30:45.123Z",
  "level": "INFO",
  "message": "Market order placed successfully",
  "data": {
    "symbol": "BTCUSDT",
    "side": "BUY",
    "quantity": "0.01",
    "order_id": "12345678",
    "status": "FILLED"
  }
}
```

## Error Handling

The bot will detect and report:
Missing API credentials
Invalid symbols or inputs
Network issues
Binance API rate limits
Insufficient balance


## Important Notes

Testnet Only – This bot is configured to operate exclusively on Binance Testnet USDT-M Futures.
No Real Money – All orders are simulated with testnet funds; no real assets are at risk.
OCO in Futures – Binance USDT-M Futures does not have native OCO order support. The bot mimics OCO by placing a limit and stop order separately, but manual cancellation of the remaining order is required if one triggers.
Rate Limits – Binance imposes request rate limits; exceeding them may temporarily block your API access.


## Dependencies
- `binance-futures-connector>=3.6.0`: Official Binance futures connector
- `python-dotenv>=1.0.0`: Environment variable loader


## Testing

To test the bot functionality:
Ensure API credentials are set
Execute test orders with small quantities using the CLI commands above

