<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1" />
<title>EUR/USD Trading Demo - SMC & ICT Concepts</title>
<style>
  body {
    font-family: Arial, sans-serif;
    margin: 0; padding: 0;
    background: #111;
    color: #eee;
    display: flex;
    flex-direction: column;
    height: 100vh;
  }
  #container {
    display: flex;
    flex: 1;
    overflow: hidden;
  }
  #chart {
    flex: 2;
    min-width: 0;
  }
  #commentary {
    flex: 1;
    background: #222;
    padding: 15px;
    overflow-y: auto;
    font-size: 1rem;
    line-height: 1.5;
  }
  #controls {
    background: #222;
    padding: 10px;
    display: flex;
    justify-content: center;
    gap: 15px;
  }
  button {
    background: #444;
    border: none;
    color: #eee;
    padding: 8px 16px;
    font-size: 1rem;
    cursor: pointer;
    border-radius: 4px;
  }
  button:hover {
    background: #666;
  }
  .buy-marker {
    color: lime;
    font-weight: bold;
  }
  .sell-marker {
    color: tomato;
    font-weight: bold;
  }
</style>
</head>
<body>

<h2 style="text-align:center; margin: 10px 0;">EUR/USD Trading Demo - SMC & ICT Concepts</h2>

<div id="container">
  <div id="chart"></div>
  <div id="commentary">
    <h3>Trading Commentary</h3>
    <div id="comment-text">Press Play to start the trading session replay.</div>
  </div>
</div>

<div id="controls">
  <button id="play">Play ▶️</button>
  <button id="pause">Pause ⏸️</button>
  <button id="restart">Restart 🔄</button>
</div>

<!-- TradingView Lightweight Charts CDN -->
<script src="https://unpkg.com/lightweight-charts/dist/lightweight-charts.standalone.production.js"></script>

<script>
// Sample EUR/USD candle data (one candle per minute, simplified)
// Format: { time: UNIX timestamp (seconds), open, high, low, close }
const candles = [
  { time: 1691625600, open: 1.0810, high: 1.0830, low: 1.0800, close: 1.0820 },
  { time: 1691625660, open: 1.0820, high: 1.0840, low: 1.0815, close: 1.0835 },
  { time: 1691625720, open: 1.0835, high: 1.0850, low: 1.0825, close: 1.0845 },
  { time: 1691625780, open: 1.0845, high: 1.0860, low: 1.0840, close: 1.0855 },
  { time: 1691625840, open: 1.0855, high: 1.0870, low: 1.0850, close: 1.0860 },
  { time: 1691625900, open: 1.0860, high: 1.0875, low: 1.0855, close: 1.0870 },
  { time: 1691625960, open: 1.0870, high: 1.0880, low: 1.0860, close: 1.0875 },
  { time: 1691626020, open: 1.0875, high: 1.0890, low: 1.0870, close: 1.0885 },
  { time: 1691626080, open: 1.0885, high: 1.0895, low: 1.0880, close: 1.0890 },
  { time: 1691626140, open: 1.0890, high: 1.0900, low: 1.0885, close: 1.0895 }
];

// Trades data - each trade marks candle index, type (buy/sell), and commentary index
const trades = [
  { candleIndex: 2, type: 'buy', commentIndex: 1 },
  { candleIndex: 4, type: 'sell', commentIndex: 2 },
  { candleIndex: 6, type: 'buy', commentIndex: 3 },
  { candleIndex: 7, type: 'sell', commentIndex: 4 },
  { candleIndex: 9, type: 'buy', commentIndex: 5 }
];

// Commentary text corresponding to trades and general notes
const commentary = [
  "Starting the session: Let's analyze the EUR/USD using multiple timeframes and SMC/ICT concepts.",
  "Trade 1: Buy at candle 3 after spotting a demand zone and confirmation from RSI and volume increase.",
  "Trade 2: Sell at candle 5 following a liquidity grab and bearish divergence on the 4H timeframe.",
  "Trade 3: Buy at candle 7 as price breaks structure, confirmed by an order block and bullish momentum.",
  "Trade 4: Sell at candle 8 on pullback to a supply zone, validated by bearish RSI and volume decrease.",
  "Trade 5: Buy at candle 10 as market shows accumulation and a bullish reversal pattern forms."
];

// Initialize chart
const chart = LightweightCharts.createChart(document.getElementById('chart'), {
  width: window.innerWidth * 0.66,
  height: window.innerHeight * 0.8,
  layout: {
    backgroundColor: '#111',
    textColor: 'white',
  },
  grid: {
    vertLines: { color: '#333' },
    horzLines: { color: '#333' },
  },
  crosshair: {
    mode: LightweightCharts.CrosshairMode.Normal,
  },
  rightPriceScale: {
    borderColor: '#555',
  },
  timeScale: {
    borderColor: '#555',
    timeVisible: true,
    secondsVisible: true,
  },
});

const candleSeries = chart.addCandlestickSeries();
candleSeries.setData([]);

// Commentary and animation logic
let currentCandle = 0;
let timer = null;

function displayComment(index) {
  document.getElementById('comment-text').innerText = commentary[index];
}

function plotTrades(upToIndex) {
  // Clear markers first
  candleSeries.setMarkers([]);
  const markers = [];

  for (const trade of trades) {
    if (trade.candleIndex <= upToIndex) {
      const c = candles[trade.candleIndex];
      markers.push({
        time: c.time,
        position: trade.type === 'buy' ? 'belowBar' : 'aboveBar',
        color: trade.type === 'buy' ? 'lime' : 'red',
        shape: trade.type === 'buy' ? 'arrowUp' : 'arrowDown',
        text: trade.type.toUpperCase(),
      });
    }
  }
  candleSeries.setMarkers(markers);
}

function animate() {
  if (currentCandle > candles.length) {
    clearInterval(timer);
    return;
  }
  candleSeries.setData(candles.slice(0, currentCandle));
  plotTrades(currentCandle-1);

  // Show commentary if there's a trade on this candle
  const tradeHere = trades.find(t => t.candleIndex === currentCandle-1);
  if (tradeHere) {
    displayComment(tradeHere.commentIndex);
  } else if (currentCandle === 0) {
    displayComment(0);
  }

  currentCandle++;
}

function play() {
  if(timer) return; // already playing
  timer = setInterval(animate, 1000);
}

function pause() {
  if(timer) {
    clearInterval(timer);
    timer = null;
  }
}

function restart() {
  pause();
  currentCandle = 0;
  candleSeries.setData([]);
  candleSeries.setMarkers([]);
  displayComment(0);
}

document.getElementById('play').addEventListener('click', play);
document.getElementById('pause').addEventListener('click', pause);
document.getElementById('restart').addEventListener('click', restart);

// Initial setup
restart();

// Resize chart on window resize
window.addEventListener('resize', () => {
  chart.applyOptions({ width: window.innerWidth * 0.66, height: window.innerHeight * 0.8 });
});
</script>
</body>
</html>
