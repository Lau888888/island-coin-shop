<!DOCTYPE html>
<html lang="zh">
<head>
  <meta charset="UTF-8">
  <title>KAI岛币商店</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background: linear-gradient(to right, #e0f7fa, #b3e5fc);
      margin: 0;
      padding: 0;
    }
    header {
      background: #0288d1;
      color: white;
      padding: 20px;
      text-align: center;
      font-size: 24px;
      font-weight: bold;
    }
    .container {
      max-width: 600px;
      margin: 30px auto;
      background: white;
      padding: 20px;
      border-radius: 12px;
      box-shadow: 0 4px 8px rgba(0,0,0,0.1);
    }
    label {
      font-weight: bold;
      margin-top: 10px;
      display: block;
    }
    input, select {
      width: 100%;
      padding: 8px;
      margin: 8px 0 15px;
      border-radius: 6px;
      border: 1px solid #ccc;
    }
    .toggle {
      display: flex;
      justify-content: center;
      margin-bottom: 20px;
    }
    .toggle button {
      flex: 1;
      padding: 10px;
      border: none;
      cursor: pointer;
      font-size: 16px;
    }
    .toggle button.active {
      background: #0288d1;
      color: white;
      font-weight: bold;
    }
    .price-box {
      background: #f1f8e9;
      padding: 10px;
      border-radius: 6px;
      margin-bottom: 15px;
      font-size: 16px;
    }
    .btn {
      background: #0288d1;
      color: white;
      padding: 10px;
      border: none;
      border-radius: 6px;
      cursor: pointer;
      width: 100%;
      font-size: 16px;
    }
    .btn:hover {
      background: #0277bd;
    }
    .order-box {
      margin-top: 20px;
      padding: 15px;
      background: #e3f2fd;
      border-radius: 8px;
      display: none;
    }
    footer {
      text-align: center;
      padding: 15px;
      font-size: 14px;
      color: #555;
    }
    .qr {
      text-align: center;
      margin-top: 10px;
    }
    .qr img {
      width: 150px;
      height: 150px;
      background: #ccc;
    }
  </style>
</head>
<body>
  <header>KAI岛币商店</header>
  <div class="container">
    <div class="toggle">
      <button id="buyBtn" class="active">购买订单</button>
      <button id="sellBtn">回收订单</button>
    </div>

    <label>游戏ID</label>
    <input type="text" id="gameId" placeholder="例如：123456">
    
    <label>游戏昵称</label>
    <input type="text" id="nickname" placeholder="游戏昵称">

    <label>数量（亿）</label>
    <input type="number" id="amount" placeholder="输入数量" min="1">

    <div class="price-box" id="priceBox">预计金额：RM 0</div>

    <button class="btn" onclick="generateOrder()">确认下单</button>

    <div class="order-box" id="orderBox"></div>

    <div class="qr">
      <p>请使用 TNG 转账</p>
      <img src="https://via.placeholder.com/150" alt="TNG QR 占位">
    </div>
  </div>

  <footer>
    📞 电话：601111153168<br>
    ⏰ 工作时间：10:00 - 22:00<br><br>
    <strong>声明：</strong> 此网站仅用于买卖 Blockman Go - Sky Block 中的游戏币
  </footer>

  <script>
    let mode = "buy"; // 默认购买

    const buyBtn = document.getElementById("buyBtn");
    const sellBtn = document.getElementById("sellBtn");
    const priceBox = document.getElementById("priceBox");
    const amountInput = document.getElementById("amount");
    const orderBox = document.getElementById("orderBox");

    buyBtn.onclick = () => {
      mode = "buy";
      buyBtn.classList.add("active");
      sellBtn.classList.remove("active");
      updatePrice();
    };

    sellBtn.onclick = () => {
      mode = "sell";
      sellBtn.classList.add("active");
      buyBtn.classList.remove("active");
      updatePrice();
    };

    amountInput.oninput = updatePrice;

    function getPrice(amount) {
      if (mode === "buy") {
        if (amount >= 1 && amount <= 9) return amount * 4;
        if (amount >= 10 && amount <= 49) return amount * 3.9;
        if (amount >= 50 && amount <= 99) return amount * 3.8;
        if (amount >= 100) return amount * 3.5;
      } else {
        return amount * 3; // 回收价 RM3/亿
      }
      return 0;
    }

    function updatePrice() {
      const amount = parseInt(amountInput.value) || 0;
      const price = getPrice(amount);
      const label = mode === "buy" ? "预计金额" : "预计获得";
      priceBox.textContent = `${label}：RM ${price.toFixed(2)}`;
    }

    function generateOrder() {
      const id = document.getElementById("gameId").value.trim();
      const nick = document.getElementById("nickname").value.trim();
      const amount = parseInt(amountInput.value) || 0;
      const price = getPrice(amount);

      if (!id || !nick || amount <= 0) {
        alert("请填写完整信息！");
        return;
      }

      const timestamp = new Date().toISOString().replace(/[-:T]/g, "").slice(0, 14);
      const orderId = "ORDER-" + timestamp;

      orderBox.style.display = "block";
      orderBox.innerHTML = `
        <strong>订单编号：</strong> ${orderId}<br>
        <strong>订单类型：</strong> ${mode === "buy" ? "购买订单" : "回收订单"}<br>
        <strong>游戏ID：</strong> ${id}<br>
        <strong>游戏昵称：</strong> ${nick}<br>
        <strong>数量：</strong> ${amount} 亿<br>
        <strong>${mode === "buy" ? "需支付金额" : "预计获得金额"}：</strong> RM ${price.toFixed(2)}<br>
        <br><em>请在 10 分钟内完成 TNG 转账。</em>
      `;
    }
  </script>
</body>
</html>
# island-coin-shop
...
