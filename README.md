# Hyperliquid Data Fetcher 🌙

A Python script for fetching historical cryptocurrency data from the Hyperliquid exchange API, designed with MoonDev's signature style and functionality.

## Overview

This script fetches historical OHLCV (Open, High, Low, Close, Volume) data for cryptocurrency trading pairs from the Hyperliquid exchange. It handles API rate limits, timestamp corrections, and saves the data in CSV format for analysis.

## Features

- 📊 **OHLCV Data Fetching**: Retrieves comprehensive candlestick data
- ⏱️ **Timestamp Correction**: Automatically handles API timestamp offset issues
- 🔄 **Retry Logic**: Built-in retry mechanism for failed API requests
- 💾 **CSV Export**: Saves data with timestamp-based filenames
- 🎯 **Configurable Parameters**: Easy to modify symbol, timeframe, and data limits
- 📈 **Data Preview**: Shows first and last 10 rows of fetched data

## Dependencies

```python
import pandas as pd
import requests
from datetime import datetime, timedelta
import numpy as np
import time
from pathlib import Path
```

## Configuration

### Default Settings

- **Symbol**: `BTC` (Bitcoin)
- **Timeframe**: `1m` (1-minute candles)
- **Batch Size**: `5000` (Maximum allowed by Hyperliquid API)
- **Max Retries**: `3`
- **Max Rows**: `5000` (Limits the number of rows returned)

### Key Constants

```python
BATCH_SIZE = 5000  # Maximum batch size for Hyperliquid API
MAX_RETRIES = 3    # Maximum retry attempts for failed requests
MAX_ROWS = 5000    # Maximum number of rows to fetch
```

## Functions

### [`adjust_timestamp(dt)`](hyperliquid_data.py:29)
Adjusts API timestamps by subtracting the calculated timestamp offset to correct for API timing issues.

**Parameters:**
- `dt`: datetime object to be adjusted

**Returns:** Adjusted datetime object

### [`get_ohlcv2(symbol, interval, start_time, end_time, batch_size)`](hyperliquid_data.py:38)
Fetches OHLCV data from the Hyperliquid API with retry logic and timestamp correction.

**Parameters:**
- `symbol`: Trading pair symbol (e.g., 'BTC')
- `interval`: Timeframe interval (e.g., '1m')
- `start_time`: Start datetime for data range
- `end_time`: End datetime for data range
- `batch_size`: Number of candles to fetch per request

**Returns:** List of candle data or None if failed

### [`process_data_to_df(snapshot_data)`](hyperliquid_data.py:105)
Converts raw API response data into a pandas DataFrame with proper column formatting.

**Parameters:**
- `snapshot_data`: Raw API response data

**Returns:** pandas DataFrame with OHLCV columns

### [`fetch_historical_data(symbol, timeframe)`](hyperliquid_data.py:125)
Main function that orchestrates the data fetching process for historical data.

**Parameters:**
- `symbol`: Trading pair symbol
- `timeframe`: Candle timeframe

**Returns:** pandas DataFrame with historical OHLCV data

## API Endpoints

The script uses the Hyperliquid API endpoint:
```
https://api.hyperliquid.xyz/info
```

**Request Type:** `candleSnapshot`
**Request Format:**
```json
{
    "type": "candleSnapshot",
    "req": {
        "coin": "BTC",
        "interval": "1m",
        "startTime": 1234567890000,
        "endTime": 1234567899999,
        "limit": 5000
    }
}
```

## Data Structure

The fetched data is structured as a pandas DataFrame with the following columns:

| Column | Type | Description |
|--------|------|-------------|
| timestamp | datetime | Candle timestamp in UTC |
| open | float | Opening price |
| high | float | Highest price during interval |
| low | float | Lowest price during interval |
| close | float | Closing price |
| volume | float | Trading volume |

## Output Files

Data is saved as CSV files with the naming convention:
```
{SYMBOL}_{TIMEFRAME}_{YYYYMMDDHHMMSS}_historical.csv
```

Example: `BTC_1m_20251125_092500_historical.csv`

## Usage

1. **Basic Usage**: Run the script directly to fetch BTC 1-minute data
2. **Modify Parameters**: Change the `symbol` and `timeframe` variables at the top of the file
3. **Custom Date Range**: Modify the `start_time` and `end_time` in the `fetch_historical_data()` function

## Error Handling

- **HTTP Errors**: Displays error codes and response text
- **Request Failures**: Implements retry logic with 1-second delays
- **No Data**: Handles cases where API returns empty responses
- **Timestamp Issues**: Automatically calculates and applies timestamp offsets

## Console Output

The script provides detailed console output including:
- 📁 Data directory location
- 🔍 Request details (batch size, time range)
- ✨ Number of candles received
- 📈 First and last candle timestamps
- 📊 Final data summary
- 💾 File save location
- 🌙 Data preview (first and last 10 rows)

## Notes

- Hyperliquid API has a maximum batch size limit of 5000 candles
- For larger datasets, consider using Coinbase API as mentioned in the code
- The script includes timestamp offset calculation to handle API timing discrepancies
- Data is automatically sorted by timestamp and limited to the most recent 5000 rows

## MoonDev Signature

This script features MoonDev's signature style with emojis and clear console output formatting, making it easy to track the data fetching process and results.

---
*🌙 Powered by MoonDev's Data Fetcher* ✨