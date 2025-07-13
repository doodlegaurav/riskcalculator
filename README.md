<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Unified Risk & Capital Calculator</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      margin: 0;
      padding: 15px;
      background: #1e1e1e;
      color: white;
    }

    h2 {
      font-size: 20px;
      text-align: center;
      margin-bottom: 20px;
    }

    label {
      font-size: 14px;
      display: block;
      margin-top: 10px;
    }

    input, button {
      width: 100%;
      padding: 10px;
      margin-top: 5px;
      border-radius: 6px;
      border: none;
      font-size: 16px;
      box-sizing: border-box;
    }

    input {
      background: #333;
      color: white;
    }

    button {
      background: #4CAF50;
      color: white;
      margin-top: 15px;
      cursor: pointer;
    }

    .result {
      margin-top: 20px;
      background: #2a2a2a;
      padding: 15px;
      border-radius: 6px;
      font-size: 14px;
      line-height: 1.5;
    }
  </style>
</head>
<body>
  <h2>📊 Trade Risk & Capital Calculator</h2>

  <label for="entry">Entry Price</label>
  <input type="number" id="entry" placeholder="e.g. 152.40">

  <label for="sl">Stop Loss Price</label>
  <input type="number" id="sl" placeholder="e.g. 149.80">

  <label for="risk">Risk per Trade (₹)</label>
  <input type="number" id="risk" value="1000">

  <label for="leverage">Leverage (e.g. 1 for cash, 2 for 2x)</label>
  <input type="number" id="leverage" value="1">

  <button onclick="calculate()">Calculate</button>

  <div class="result" id="result"></div>

  <script>
    function calculate() {
      const entry = parseFloat(document.getElementById("entry").value);
      const sl = parseFloat(document.getElementById("sl").value);
      const risk = parseFloat(document.getElementById("risk").value);
      const leverage = parseFloat(document.getElementById("leverage").value || 1);

      const stopSize = Math.abs(entry - sl);

      if (!entry || !sl || !risk || stopSize === 0 || leverage <= 0) {
        document.getElementById("result").innerHTML = "❌ Please enter valid values.";
        return;
      }

      const qty = Math.floor(risk / stopSize);
      const target1 = entry > sl ? entry + stopSize * 1.5 : entry - stopSize * 1.5;
      const target2 = entry > sl ? entry + stopSize * 3 : entry - stopSize * 3;
      const totalValue = entry * qty;
      const requiredCapital = totalValue / leverage;

      document.getElementById("result").innerHTML = `
        🛑 <strong>Stop Size:</strong> ₹${stopSize.toFixed(2)}<br>
        📦 <strong>Quantity:</strong> ${qty} shares<br>
        🎯 <strong>1:1.5 Target:</strong> ₹${target1.toFixed(2)}<br>
        🎯 <strong>1:3 Target:</strong> ₹${target2.toFixed(2)}<br>
        💼 <strong>Total Trade Value:</strong> ₹${totalValue.toFixed(2)}<br>
        🏦 <strong>Capital Required (Leverage ${leverage}x):</strong> ₹${requiredCapital.toFixed(2)}
      `;
    }
  </script>
</body>
</html>
