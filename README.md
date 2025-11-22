myshop/
├─ package.json
├─ server.js
├─ orders.json          # automatically created
├─ uploads/             # screenshots saved here
└─ public/
   ├─ index.html
   └─ styles.css

<!DOCTYPE html>
<html lang="hi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Online Store – Order Now</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background:#f2f2f2;
            margin:0; padding:0;
        }
        .header {
            background:#6a0dad;
            color:white;
            padding:15px;
            text-align:center;
            font-size:25px;
            font-weight:bold;
        }
        .product-box, .order-box, .payment-box {
            background:white;
            width:90%;
            margin:20px auto;
            padding:20px;
            border-radius:10px;
            box-shadow:0px 0px 8px #bbb;
        }
        .product-card {
            border:1px solid #ccc;
            border-radius:10px;
            padding:15px;
            margin-bottom:15px;
        }
        button {
            background:#6a0dad;
            color:white;
            padding:10px 20px;
            border:none;
            border-radius:5px;
            cursor:pointer;
        }
        button:hover { background:#4c067c; }
        input, select {
            width:100%;
            padding:10px;
            margin:8px 0;
            border-radius:5px;
            border:1px solid #999;
        }
        img {
            max-width:100%;
        }
    </style>
</head>
<body>

<div class="header">Online Shopping Store</div>

<!-- PRODUCTS -->
<div class="product-box">
    <h2>🛍️ Products</h2>

    <div class="product-card">
        <h3>Shirt</h3>
        <p>Price: ₹450</p>
        <button onclick="orderNow('Shirt', 450)">Order</button>
    </div>

    <div class="product-card">
        <h3>Pant</h3>
        <p>Price: ₹650</p>
        <button onclick="orderNow('Pant', 650)">Order</button>
    </div>

    <div class="product-card">
        <h3>Fan</h3>
        <p>Price: ₹1200</p>
        <button onclick="orderNow('Fan', 1200)">Order</button>
    </div>

    <div class="product-card">
        <h3>Book</h3>
        <p>Price: ₹120</p>
        <button onclick="orderNow('Book', 120)">Order</button>
    </div>
</div>

<!-- ORDER FORM -->
<div class="order-box" id="orderForm" style="display:none;">
    <h2>📝 Order Form</h2>
    <form id="finalOrderForm">

        <label>Customer Name</label>
        <input type="text" required>

        <label>Mobile Number</label>
        <input type="number" required>

        <label>PIN Code</label>
        <input type="number" required>

        <label>Village</label>
        <input type="text" required>

        <label>District</label>
        <input type="text" required>

        <label>Full Address</label>
        <input type="text" required>

        <label>Product</label>
        <input type="text" id="productName" readonly>

        <label>Price (₹)</label>
        <input type="text" id="productPrice" readonly>

        <label>Quantity</label>
        <input type="number" value="1" min="1" required>

        <br><br>

        <h3>📦 Delivery</h3>
        <p>डिलीवरी समय: <b>15 दिन</b></p>

        <h3>💰 Payment</h3>
        <p><b>UPI ID:</b> 7544985606-3@ybl</p>

        <h3>Scan & Pay</h3>
        <img src="qr.png" alt="QR Code" style="width:250px;">

        <label>Payment Screenshot Upload</label>
        <input type="file" accept="image/*" required>

        <br><br>
        <button type="submit">Confirm Order</button>
    </form>
</div>

<script>
function orderNow(name, price) {
    document.getElementById('orderForm').style.display = 'block';
    document.getElementById('productName').value = name;
    document.getElementById('productPrice').value = price;
}
</script>

</body>
</html>

<h2>Pay via UPI</h2>
<p>Scan this QR to pay:</p>

<img 
  src="https://api.qrserver.com/v1/create-qr-code/?size=260x260&data=upi://pay?pa=7544985606-3@ybl&pn=Your+Shop&cu=INR" 
  alt="UPI QR Code"
/>

<p><b>UPI ID:</b> 7544985606-3@ybl</p>
<p>Payment direct aapke UPI me aayega.</p>

// server.js
const express = require("express");
const bodyParser = require("body-parser");
const { v4: uuid } = require("uuid");

const app = express();
app.use(bodyParser.json());
app.use(express.static(__dirname));  // serve your index.html

const ORDERS = {};

app.post("/create-order", (req, res) => {
  try {
    const { customer, items, total } = req.body;

    if (!customer || !items || items.length === 0) {
      return res.json({ success: false, message: "Invalid order" });
    }

    const orderId = "ORD-" + uuid().slice(0, 8).toUpperCase();

    ORDERS[orderId] = {
      id: orderId,
      customer,
      items,
      total,
      status: "paid",     // simulated payment
      createdAt: new Date(),
    };

    return res.json({
      success: true,
      orderId,
      paymentStatus: "success",
    });
  } catch (err) {
    console.log(err);
    res.json({ success: false, message: "Server error" });
  }
});

app.listen(3000, () =>
  console.log("Server running: http://localhost:3000")
);

https://upload.meeshosupplyassets.com/cataloging/1763736924208/1000000741.jpg