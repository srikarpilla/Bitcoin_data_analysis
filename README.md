# Bitcoin Market Sentiment vs Trader Performance

## What this is
This project checks whether Hyperliquid trader performance (PnL, win rate,
position size, long/short bias) changes with Bitcoin market sentiment
(Fear/Greed Index).

## Files
- `analysis.py` - main script, run this
- `fear_greed_index.xlsx` - daily sentiment data (date, classification, value)
- `historical_data.csv` - trade level data (account, coin, price, size, side, pnl, etc.)
- `outputs/` - generated after running the script
  - `merged_trades_with_sentiment.csv` - trades joined with sentiment by date
  - `pnl_summary_by_sentiment.csv` - PnL and win rate grouped by sentiment
  - `behaviour_by_sentiment.csv` - avg trade size, fees, long % by sentiment
  - `account_pnl_by_sentiment.csv` - PnL per account per sentiment regime
  - 6 chart PNGs (boxplot, win rate bar, long bias bar, size bar,
    cumulative PnL line, account x sentiment heatmap)

## How to run
```
pip install pandas numpy matplotlib seaborn scipy openpyxl
python analysis.py
```
Make sure the two data files are in the same folder as `analysis.py`.

## Approach
1. Load both files, convert timestamps to plain dates.
2. Merge trades onto sentiment using the date as key.
3. Group by sentiment (Extreme Fear, Fear, Neutral, Greed, Extreme Greed)
   and compute PnL, win rate, average trade size, and long/short ratio.
4. Run a Mann-Whitney U test to check if Fear PnL is statistically
   different from Greed PnL.
5. Break it down further by account and by coin to see if the sentiment
   effect is consistent across traders or only a few.
6. Plot everything and save summary tables.

## Key findings
- Win rate is highest during Neutral/Greed (~92-95%) but drops sharply
  in Extreme Greed (~52%) - traders keep winning as greed builds but
  get caught out once it peaks.
- Average trade size is much smaller during Extreme Fear/Extreme Greed
  (~$7-8K) vs Fear/Greed (~$25-30K), meaning traders naturally reduce
  size at emotional extremes.
- Fear vs Greed PnL difference is not statistically significant overall
  (Mann-Whitney p = 0.60), so the effect shows up mainly at the extremes,
  not on average days.
- Performance is not uniform across accounts - some traders swing from
  100% win rate in Extreme Fear to 26% in Extreme Greed, while one
  account stays consistently profitable across all regimes.

## Takeaway for strategy
Cap leverage/position size during Extreme Greed since that's where win
rate collapses. Extreme Fear performance held up well, so contrarian
entries during fear may be worth testing further.
