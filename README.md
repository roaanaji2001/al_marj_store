<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
  <meta charset="UTF-8">
  <title>متجر المرج</title>
  <style>
    body { font-family: 'Arial', sans-serif; margin:0; padding:0; background:#f7f7f7; }
    header { background:#222; color:#fff; padding:20px; text-align:center; font-size:22px; }
    nav { background:#333; padding:10px; color:#fff; display:flex; gap:15px; align-items:center; flex-wrap:wrap; }
    nav a { color:#fff; text-decoration:none; font-size:17px; padding:5px 10px; border-radius:4px; }
    nav a:hover { background:#555; }
    nav input[type="search"] { padding:7px; font-size:16px; border-radius:4px; border:none; width:200px; }
    .products { display:flex; flex-wrap:wrap; gap:20px; padding:20px; justify-content:center; }
    .product { background:#fff; border:1px solid #ddd; width:220px; padding:10px; border-radius:6px; text-align:center; }
    .product img { width:100%; border-radius:4px; }
    .product h3 { font-size:18px; margin:8px 0; }
    .product p { margin:5px 0; font-size:16px; }
    .product button { padding:7px 10px; background:#1e88e5; color:#fff; border:none; border-radius:4px; cursor:pointer; }
    .product button:hover { background:#0d6cd4; }

    .cart {
      position:fixed; top:20px; left:20px;
      background:#fff; border:1px solid #ccc;
      padding:15px; border-radius:6px;
      width:260px; max-height:80vh; overflow-y:auto;
    }
    .cart-item { border-bottom:1px solid #eee; padding:6px 0; font-size:15px; }
    .cart-item button { float:left; background:red; color:#fff; border:none; padding:2px 7px; }

    .overlay {
      display:none; position:fixed; top:0; left:0;
      width:100%; height:100%; background:rgba(0,0,0,0.6);
    }
    .checkout-form {
      display:none; position:fixed; top:50%; left:50%;
      transform:translate(-50%, -50%);
      background:#fff; padding:20px; border-radius:6px;
      width:300px; z-index:50;
    }
    .checkout-form input { width:100%; padding:8px; margin:6px 0; }
    .checkout-form button { padding:10px; width:100%; margin-top:8px; }
  </style>
</head>

<body>

<header>متجر المــرج</header>

<nav>
  <a href="#" onclick="filterCategory('clothes')">ملابس 🛍️</a>
  <a href="#" onclick="filterCategory('makeup')">مكياج 💄</a>
  <a href="#" onclick="filterCategory('accessories')">إكسسوارات 💍</a>

  <input type="search" id="searchBox" placeholder="ابحث…" oninput="searchProducts()">
</nav>

<div class="products" id="products"></div>

<div class="cart" id="cart">
  <h3>السلة</h3>
  <div id="cart-items"></div>
  <p>الإجمالي: <span id="total">0</span> د.ل</p>
  <button onclick="showCheckout()">تأكيد الطلب</button>
</div>

<div class="overlay" id="overlay"></div>
<div class="checkout-form" id="checkout-form">
  <h3>بيانات الدفع</h3>
  <input type="text" id="customer-name" placeholder="الاسم الكامل">
  <input type="text" id="customer-phone" placeholder="رقم الهاتف">
  <input type="text" id="customer-address" placeholder="العنوان">
  <button onclick="placeOrder()">إرسال الطلب</button>
  <button onclick="closeCheckout()">إغلاق</button>
</div>

<script>
  const productsList = [
    { id:1, name:"فستان أحمر", price:20, category:"clothes", img:"https://via.placeholder.com/300" },
    { id:2, name:"روج ثابت", price:10, category:"makeup", img:"https://via.placeholder.com/300" },
    { id:3, name:"إسوارة ذهبية", price:15, category:"accessories", img:"https://via.placeholder.com/300" }
  ];

  let cart = [];

  function renderProducts(list) {
    document.getElementById('products').innerHTML = list.map(p=>`
      <div class="product">
        <img src="${p.img}">
        <h3>${p.name}</h3>
        <p>السعر: ${p.price} د.ل</p>
        <button onclick="addToCart(${p.id})">أضف للسلة</button>
      </div>
    `).join('');
  }

  function addToCart(id) {
    const product = productsList.find(p => p.id === id);
    cart.push(product);
    renderCart();
  }

  function renderCart() {
    document.getElementById('cart-items').innerHTML = cart.map((item, index)=>`
      <div class="cart-item">
        ${item.name} — ${item.price} د.ل
        <button onclick="removeItem(${index})">x</button>
      </div>
    `).join('');

    const total = cart.reduce((sum,i)=> sum + i.price, 0);
    document.getElementById('total').innerText = total;
  }

  function removeItem(i) {
    cart.splice(i,1);
    renderCart();
  }

  function filterCategory(cat) {
    renderProducts(productsList.filter(p => p.category === cat));
  }

  function searchProducts() {
    const t = document.getElementById('searchBox').value;
    renderProducts(productsList.filter(p=> p.name.includes(t)));
  }

  function showCheckout() {
    document.getElementById('overlay').style.display="block";
    document.getElementById('checkout-form').style.display="block";
  }
  function closeCheckout() {
    document.getElementById('overlay').style.display="none";
    document.getElementById('checkout-form').style.display="none";
  }

  function placeOrder() {
    const name = document.getElementById('customer-name').value;
    const phone = document.getElementById('customer-phone').value;
    if(!name || !phone){ alert("ادخلي الاسم ورقم الهاتف"); return; }
    alert("تم استلام طلبك 🌸");
    closeCheckout();
  }

  renderProducts(productsList);
</script>

</body>
</html>