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
      line-height: 1.6;
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
    function formatIndian(num) {
      return num.toLocaleString('en-IN', {
        maximumFractionDigits: 2,
        minimumFractionDigits: 2
      });
    }

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
      const target1 = entry > sl ? entry + stopSize * 1 : entry - stopSize * 1;
      const target2 = entry > sl ? entry + stopSize * 2 : entry - stopSize * 2;
      const target3 = entry > sl ? entry + stopSize * 3 : entry - stopSize * 3;
      const totalValue = entry * qty;
      const requiredCapital = totalValue / leverage;

      document.getElementById("result").innerHTML = `
        🛑 <strong>Stop Size:</strong> ₹${formatIndian(stopSize)}<br>
        📦 <strong>Quantity:</strong> ${qty} shares<br><br>
        🎯 <strong>Target 1 (1:1):</strong> ₹${formatIndian(target1)}<br>
        🎯 <strong>Target 2 (1:2):</strong> ₹${formatIndian(target2)}<br>
        🎯 <strong>Target 3 (1:3):</strong> ₹${formatIndian(target3)}<br><br>
        💼 <strong>Total Trade Value:</strong> ₹${formatIndian(totalValue)}<br>
        🏦 <strong>Capital Required (Leverage ${leverage}x):</strong> ₹${formatIndian(requiredCapital)}
      `;
    }
  </script>
</body>
</html>
