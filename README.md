<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>متجر أوريفليم</title>
    <style>
        body {
            font-family: 'Arial', sans-serif;
            margin: 0;
            padding: 0;
            background-color: #f4f4f4;
            color: #333;
        }
        header {
            background-color: #e91e63;
            color: white;
            padding: 20px;
            text-align: center;
        }
        .container {
            max-width: 1200px;
            margin: 20px auto;
            padding: 20px;
            background: white;
            border-radius: 8px;
            box-shadow: 0 0 10px rgba(0,0,0,0.1);
        }
        .products {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
            gap: 20px;
        }
        .product {
            border: 1px solid #ddd;
            border-radius: 8px;
            padding: 15px;
            text-align: center;
        }
        .product img {
            max-width: 100%;
            height: 200px;
            object-fit: cover;
            border-radius: 4px;
        }
        .product h3 {
            margin: 10px 0;
        }
        .product p {
            color: #e91e63;
            font-weight: bold;
        }
        .order-btn {
            background-color: #4CAF50;
            color: white;
            border: none;
            padding: 10px 20px;
            border-radius: 4px;
            cursor: pointer;
            margin-top: 10px;
        }
        .order-btn:hover {
            background-color: #45a049;
        }
        .admin-section {
            margin-top: 40px;
            border-top: 2px solid #e91e63;
            padding-top: 20px;
        }
        .admin-section input, .admin-section button {
            margin: 10px;
            padding: 10px;
        }
        .orders-section {
            margin-top: 40px;
            border-top: 2px solid #e91e63;
            padding-top: 20px;
        }
        .order {
            border: 1px solid #ddd;
            padding: 10px;
            margin: 10px 0;
            border-radius: 4px;
        }
        .whatsapp-link {
            display: inline-block;
            background-color: #25D366;
            color: white;
            padding: 10px 20px;
            text-decoration: none;
            border-radius: 4px;
            margin-top: 10px;
        }
        .whatsapp-link:hover {
            background-color: #128C7E;
        }
    </style>
</head>
<body>
    <header>
        <h1>متجر أوريفليم</h1>
        <p>منتجات تجميل عالية الجودة</p>
    </header>

    <div class="container">
        <h2>منتجاتنا</h2>
        <div class="products" id="products"></div>

        <div class="admin-section">
            <h2>إدارة المنتجات (للإدارة فقط)</h2>
            <input type="password" id="adminCode" placeholder="كود الإدارة">
            <button onclick="loginAdmin()">دخول</button>
            <div id="adminPanel" style="display: none;">
                <h3>إضافة منتج جديد</h3>
                <input type="text" id="productName" placeholder="اسم المنتج">
                <input type="number" id="productPrice" placeholder="السعر">
                <input type="text" id="productImage" placeholder="رابط الصورة">
                <button onclick="addProduct()">إضافة</button>
                <h3>حذف منتج</h3>
                <input type="number" id="deleteId" placeholder="رقم المنتج">
                <button onclick="deleteProduct()">حذف</button>
            </div>
        </div>

        <div class="orders-section">
            <h2>الطلبات</h2>
            <div id="orders"></div>
        </div>
    </div>

    <script>
        const adminCode = "1234"; // غير هذا الكود حسب رغبتك
        const whatsappNumber = "0764460531";

        let products = JSON.parse(localStorage.getItem('products')) || [
            { id: 1, name: "un ensemble des pinceaux magique pour sublimer votre look de fêtes 48572", price: 150, image: "p1.jpg.jpg" },
            { id: 2, name: "rouge a levers liquide paillete nude: 48640", price: 150, image: "p2.jpg.jpg" },
            { id: 3, name: "مزيل العرق المعطر جيورداني جولد ميس جيورداني (47759)", price: 150, image: "p3.jpg.jpg" },
            { id: 4, name: "عطر جيورداني جولد مسيتر جيورداني46045", price: 379, image: "p4.jpg.jpg" },
            { id: 5, name: "ombre a paupières liqyude The one   (more color). 47191", price: 180, image: "p5.jpg.jpg" },
            { id: 6, name: "eau de parfum divine dark velvet (50ml) 46801", price: 250, image: "p6.jpg.jpg" }
        ];

        let orders = JSON.parse(localStorage.getItem('orders')) || [];

        function displayProducts() {
            const productsDiv = document.getElementById('products');
            productsDiv.innerHTML = '';
            products.forEach(product => {
                productsDiv.innerHTML += `
                    <div class="product">
                        <img src="${product.image}" alt="${product.name}">
                        <h3>${product.name}</h3>
                        <p>${product.price} درهم</p>
                        <button class="order-btn" onclick="orderProduct(${product.id})">اطلب الآن</button>
                    </div>
                `;
            });
        }

        function orderProduct(productId) {
            const product = products.find(p => p.id === productId);
            const order = {
                id: Date.now(),
                product: product.name,
                price: product.price,
                date: new Date().toLocaleString()
            };
            orders.push(order);
            localStorage.setItem('orders', JSON.stringify(orders));
            displayOrders();
            alert('تم إضافة الطلب بنجاح!');
        }

        function displayOrders() {
            const ordersDiv = document.getElementById('orders');
            ordersDiv.innerHTML = '';
            orders.forEach(order => {
                ordersDiv.innerHTML += `
                    <div class="order">
                        <p><strong>المنتج:</strong> ${order.product}</p>
                        <p><strong>السعر:</strong> ${order.price} درهم</p>
                        <p><strong>التاريخ:</strong> ${order.date}</p>
                        <a href="https://wa.me/${whatsappNumber}?text=مرحبا، أريد طلب ${order.product}" class="whatsapp-link" target="_blank">إرسال عبر واتساب</a>
                    </div>
                `;
            });
        }

        function loginAdmin() {
            const code = document.getElementById('adminCode').value;
            if (code === adminCode) {
                document.getElementById('adminPanel').style.display = 'block';
            } else {
                alert('كود خاطئ!');
            }
        }

        function addProduct() {
            const name = document.getElementById('productName').value;
            const price = document.getElementById('productPrice').value;
            const image = document.getElementById('productImage').value;
            if (name && price && image) {
                const newProduct = {
                    id: products.length + 1,
                    name: name,
                    price: parseFloat(price),
                    image: image
                };
                products.push(newProduct);
                localStorage.setItem('products', JSON.stringify(products));
                displayProducts();
                document.getElementById('productName').value = '';
                document.getElementById('productPrice').value = '';
                document.getElementById('productImage').value = '';
                alert('تم إضافة المنتج بنجاح!');
            } else {
                alert('يرجى ملء جميع الحقول!');
            }
        }

        function deleteProduct() {
            const id = parseInt(document.getElementById('deleteId').value);
            products = products.filter(p => p.id !== id);
            localStorage.setItem('products', JSON.stringify(products));
            displayProducts();
            document.getElementById('deleteId').value = '';
            alert('تم حذف المنتج بنجاح!');
        }

        // تحميل البيانات عند تحميل الصفحة
        window.onload = function() {
            displayProducts();
            displayOrders();
        };
    </script>
</body>
