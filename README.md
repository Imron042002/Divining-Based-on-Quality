# Divining-Based-on-Quality
# 📊 Stock Analysis & Divining Based on Quality

This project is a Thai stock analysis system that retrieves Candlestick data from the Settrade API and stores it in Cassandra. It also visualizes a Divining Based on Quality Treemap with tooltips and a legend.

## Features
- Connects to Cassandra to store Candlestick data
- Fetches stock data from Settrade API (Sandbox/Live)
- Analyzes stock quality based on ROE, P/E, P/BV, Dividend
- Creates a Treemap sized by ROE or % Change, colored by Quality
- Tooltip shows ROE, P/E, P/BV, Dividend, Change
- Legend and annotation explain stock quality levels clearly
- Summarizes stock data by quality level (Mean, Max, Min)

## Installation
1. Clone the repository:
git clone https://github.com/<username>/stock_analysis.git
cd stock_analysis

2. Create a virtual environment:
conda create -n stock_env python=3.10
conda activate stock_env

3. Install dependencies:
pip install -r requirements.txt

## Usage
- Edit stocth_names.xlsx to add stock symbols
- Update Settrade API credentials in the code
- Run: python stock_analysis.py
- View Treemap and summary table

## Treemap Explanation
- Size: ROE or % Change
- Color: Quality (Green=Excellent, Lime=Good, Gold=Fair, Orange=Poor, Red=Unacceptable)
- Tooltip: ROE, P/E, P/BV, Dividend, Change

## Cassandra Table Schema
| Column | Type | Description |
|--------|------|-------------|
| symbol | text | Stock symbol |
| time | timestamp | Candlestick timestamp |
| open_price | float | Opening price |
| high_price | float | Highest price |
| low_price | float | Lowest price |
| close_price | float | Closing price |
| volume | bigint | Traded volume |
| value | float | Traded value |

## License
MIT License © 2025
"""
with open("README.md", "w", encoding="utf-8") as f:
    f.write(readme_content)

