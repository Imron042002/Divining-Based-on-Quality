# Divining-Based-on-Quality
# 📊 Stock Analysis & Divining Based on Quality

This system analyzes Thai stocks by fetching Candlestick data from the Settrade API and storing it in Cassandra. It evaluates stock quality and visualizes a Treemap for Divining Based on Quality.

🔹 Features

Connect to Cassandra to store Candlestick data

Fetch stock data from Settrade API (Sandbox/Live)

Analyze stock quality using ROE, P/E, P/BV, Dividend

Generate Treemap with Legend and Annotation

Summarize stocks by quality level

🛠️ Installation
pip install pandas numpy cassandra-driver plotly openpyxl settrade-v2


Cassandra must be installed and accessible at 127.0.0.1.

⚙️ Usage (Summary)
# Connect to Cassandra
cluster = Cluster(['127.0.0.1'])
session = cluster.connect()
session.set_keyspace('data_stock')

# Connect to Settrade API
investor = Investor(app_id="APP_ID", app_secret="APP_SECRET", broker_id="SANDBOX", app_code="SANDBOX")
market = investor.MarketData()

# Load stock symbols
symbols = pd.read_excel("get_stock_name/stocth_names.xlsx")['หลักทรัพย์'].dropna().tolist()

# Fetch and store Candlestick data
for s in symbols:
    res = market.get_candlestick(symbol=s, interval="1d", limit=100, normalized=True)["data"]
    for i in range(len(res["time"])):
        session.execute("""INSERT INTO candlestick_data (...) VALUES (...)""", (...))

# Analyze stock quality
data_quality = pd.DataFrame({
    "Symbol": symbols,
    "P/E": np.random.uniform(5,40,len(symbols)),
    "P/BV": np.random.uniform(0.5,5,len(symbols)),
    "ROE": np.random.uniform(2,30,len(symbols)),
    "Dividend": np.random.uniform(0,8,len(symbols))
})

def classify_quality(row):
    if row["ROE"]>20 and row["P/E"]<15 and row["P/BV"]<2: return "Excellent"
    elif row["ROE"]>15 and row["P/E"]<20: return "Good"
    elif row["ROE"]>10: return "Fair"
    elif row["ROE"]>5: return "Poor"
    else: return "Unacceptable"

data_quality["Quality"] = data_quality.apply(classify_quality, axis=1)

# Create Treemap
fig = px.treemap(data_quality, path=["Symbol"], values="ROE", color="Quality",
                 color_discrete_map={"Excellent":"green","Good":"limegreen","Fair":"gold","Poor":"orange","Unacceptable":"red"},
                 custom_data=["ROE","P/E","P/BV","Dividend"])
fig.show()

# Summarize stocks by quality
summary = data_quality.groupby("Quality").agg({"Symbol":"count","ROE":["mean","max","min"],"P/E":["mean","max","min"],"P/BV":["mean","max","min"],"Dividend":["mean","max","min"]}).reset_index()
print(summary)


![alt text](<Screenshot 2025-10-07 135725.png>)

📊 Color Legend (Treemap)

🟢 Green: Excellent

🍏 Lime: Good

🟡 Gold: Fair

🟠 Orange: Poor

🔴 Red: Unacceptable

💡 Business Model Canvas
Key Partners	Key Activities	Key Resources
- Settrade API (stock data)
- Cassandra (Database)
- Python ecosystem (pandas, plotly, numpy)	- Fetch Candlestick data from Settrade API
- Analyze stock quality
- Create Visualization (Treemap, Summary)	- Development team
- Server / Cloud for Cassandra
- Settrade API license
Value Propositions	Customer Relationships	Channels
- Analyze Thai stocks by quality
- Help investors choose good stocks
- Easy-to-read visual dashboard
- Statistical stock summaries	- Online Dashboard / Web Application
- Report / PDF summaries
- Notifications / Alerts	- Web Application
- Excel / CSV Export
- GitHub (for Open Source example)
Customer Segments	Cost Structure	Revenue Streams
- Retail investors in Thailand
- Fund managers / Financial analysts
- Students or finance learners	- Server / Database Costs (Cassandra)
- API Subscription Fees (Settrade)
- Development & Maintenance	- Subscription for Dashboard / Analytics
- Consulting / Custom Reports
- Data Access Fee

