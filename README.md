<!DOCTYPE html>
<html lang="fa" dir="rtl">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>ماشین حساب مدیریت سرمایه</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background: #f5f7fb;
      margin: 0;
      padding: 16px;
      color: #222;
    }
    .container {
      max-width: 520px;
      margin: auto;
      background: #fff;
      border-radius: 16px;
      padding: 20px;
      box-shadow: 0 6px 20px rgba(0,0,0,0.08);
    }
    h1 {
      font-size: 22px;
      margin-bottom: 16px;
      text-align: center;
      color: #0f172a;
    }
    label {
      display: block;
      margin: 14px 0 6px;
      font-weight: bold;
      color: #334155;
    }
    input {
      width: 100%;
      padding: 12px 14px;
      border: 1px solid #cbd5e1;
      border-radius: 10px;
      font-size: 16px;
      box-sizing: border-box;
      outline: none;
    }
    input:focus {
      border-color: #2563eb;
    }
    .btn {
      width: 100%;
      margin-top: 18px;
      background: #2563eb;
      color: white;
      border: none;
      border-radius: 12px;
      padding: 14px;
      font-size: 16px;
      cursor: pointer;
    }
    .btn:hover {
      background: #1d4ed8;
    }
    .result {
      margin-top: 20px;
      background: #f8fafc;
      border: 1px solid #e2e8f0;
      border-radius: 12px;
      padding: 16px;
      line-height: 1.9;
    }
    .highlight {
      color: #0f766e;
      font-weight: bold;
    }
    .small {
      font-size: 13px;
      color: #64748b;
      margin-top: 8px;
    }
  </style>
</head>
<body>
  <div class="container">
    <h1>ماشین حساب مدیریت سرمایه</h1>

    <label for="balance">موجودی حساب (دلار)</label>
    <input type="number" id="balance" value="3000" step="0.01" />

    <label for="riskPercent">درصد ریسک در هر معامله (%)</label>
    <input type="number" id="riskPercent" value="2" step="0.01" />

    <label for="stopLossPips">فاصله استاپ‌لاس (پیپ)</label>
    <input type="number" id="stopLossPips" value="20" step="0.01" />

    <label for="tpPips">فاصله تی‌پی (پیپ)</label>
    <input type="number" id="tpPips" value="40" step="0.01" />

    <label for="pipValuePerLot">ارزش هر پیپ برای 1 لات (دلار)</label>
    <input type="number" id="pipValuePerLot" value="10" step="0.01" />

    <button class="btn" onclick="calculate()">محاسبه</button>

    <div class="result" id="result">
      لطفاً اطلاعات را وارد کنید و روی «محاسبه» بزنید.
    </div>

    <div class="small">
      فرمول حجم معامله: <br>
      حجم لات = مقدار ریسک ÷ (فاصله استاپ‌لاس × ارزش هر پیپ برای 1 لات)
    </div>
  </div>

  <script>
    function calculate() {
      const balance = parseFloat(document.getElementById('balance').value) || 0;
      const riskPercent = parseFloat(document.getElementById('riskPercent').value) || 0;
      const stopLossPips = parseFloat(document.getElementById('stopLossPips').value) || 0;
      const tpPips = parseFloat(document.getElementById('tpPips').value) || 0;
      const pipValuePerLot = parseFloat(document.getElementById('pipValuePerLot').value) || 10;

      const riskAmount = balance * (riskPercent / 100);
      const lotSize = (stopLossPips > 0 && pipValuePerLot > 0)
        ? riskAmount / (stopLossPips * pipValuePerLot)
        : 0;

      const potentialProfit = tpPips * pipValuePerLot * lotSize;
      const riskReward = stopLossPips > 0 ? (tpPips / stopLossPips) : 0;

      document.getElementById('result').innerHTML = `
        <div>مقدار ریسک مجاز: <span class="highlight">$${riskAmount.toFixed(2)}</span></div>
        <div>حجم مناسب معامله: <span class="highlight">${lotSize.toFixed(2)}</span> لات</div>
        <div>سود احتمالی در تی‌پی: <span class="highlight">$${potentialProfit.toFixed(2)}</span></div>
        <div>نسبت ریسک به ریوارد: <span class="highlight">1 : ${riskReward.toFixed(2)}</span></div>
      `;
    }

    calculate();
  </script>
</body>
</html>
