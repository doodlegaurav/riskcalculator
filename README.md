<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Risk & Capital Calculator</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      margin: 0;
      padding: 15px;
      background: #1e1e1e;
      color: white;
    }

    h2 {
      font-size: 18px;
      text-align: center;
      margin-top: 30px;
      margin-bottom: 10px;
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
      margin-top: 15px;
      background: #2a2a2a;
      padding: 15px;
      border-radius: 6px;
      font-size: 14px;
      line-height: 1.5;
    }

    hr {
      border: none;
      border-top: 1px solid #444;
      margin: 30px 0;
    }
  </style>
</head>
<body>

  <!-- RISK CALCULATOR -->
  <h2>📊 Risk per Trade Calculator</h2>
  <label for="entry">Entry Price</label>
  <input type="number" id="entry" placeholder="e.g. 152.40">

  <label for="sl">Stop Loss Price</label>
  <input type="number" id="sl" placeholder="e.g. 149.80">

  <label for="risk">Risk per Trade (₹)</label>
  <input type="number" id="risk" value="1000">

  <button onclick="calculateRisk()">Calculate</button>

  <div class="result" id="result"></div>

  <hr>

  <!-- CAPITAL CALCULATOR -->
  <h2>💰 Capital Required Calculator</h2>
  <label for="entry2">Entry Price</label>
  <input type="number" id="entry2" placeholder="e.g. 200">

  <label for="qty">Quantity</label>
  <input type="number" id="qty" placeholder="e.g. 50">

  <label for="leverage">Leverage (e.g. 2 for 2x, 5 for 5x)</label>
  <input type="number" id="leverage" value="1" placeholder="e.g. 2 or 5">

  <button onclick="calculateCapital()">Calculate</button>

  <div class="result" id="result2"></div>

  <script>
    function calculateRisk() {
      const entry = parseFloat(document.getElementById("entry").value);
      const sl = parseFloat(document.getElementById("sl").value);
      const risk = parseFloat(document.getElementById("risk").value);
      const stopSize = Math.abs(entry - sl);

      if (!entry || !sl || !risk || stopSize === 0) {
        document.getElementById("result").innerHTML = "❌ Please enter valid values.";
        return;
      }

      const qty = Math.floor(risk / stopSize);
      const target = entry > sl ? entry + stopSize * 2 : entry - stopSize * 2;

      document.getElementById("result").innerHTML = `
        🛑 <strong>Stop Size:</strong> ₹${stopSize.toFixed(2)}<br>
        📦 <strong>Quantity:</strong> ${qty} shares<br>
        🎯 <strong>1:2 Target Price:</strong> ₹${target.toFixed(2)}
      `;
    }

    function calculateCapital() {
      const entry = parseFloat(document.getElementById("entry2").value);
      const qty = parseFloat(document.getElementById("qty").value);
      const leverage = parseFloat(document.getElementById("leverage").value || 1);

      if (!entry || !qty || leverage <= 0) {
        document.getElementById("result2").innerHTML = "❌ Please enter valid values.";
        return;
      }

      const totalValue = entry * qty;
      const requiredCapital = totalValue / leverage;

      document.getElementById("result2").innerHTML = `
        💼 <strong>Total Trade Value:</strong> ₹${totalValue.toFixed(2)}<br>
        🏦 <strong>Capital Required (with ${leverage}x leverage):</strong> ₹${requiredCapital.toFixed(2)}
      `;
    }
  </script>
</body>
</html>
