<!DOCTYPE html>
<html lang="ar">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Rosh Office - بيع الموبايلات بالتقسيط</title>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Noto+Naskh+Arabic&display=swap');
        body { 
            font-family: 'Noto Naskh Arabic', serif;
            background-color: #f8f9fa;
            text-align: center;
            direction: rtl;
            color: #333;
        }
        .container { 
            width: 80%; 
            margin: auto; 
            background: white; 
            padding: 20px; 
            border-radius: 10px; 
            box-shadow: 0px 4px 10px rgba(0, 0, 0, 1.1);
        }
        .product { 
            border-bottom: 1px solid #ddd; 
            padding: 15px; 
            display: flex; 
            align-items: center; 
            justify-content: space-between;
            font-size: 18px;
        }
        .product img { 
            width: 250px; 
            height: 250px; 
            margin-left: 15px; 
            border-radius: 10px; 
            box-shadow: 0px 4px 8px rgba(0, 0, 0, 0.2);
        }
        .hidden { display: none; }
        .search-bar { 
            margin-bottom: 20px; 
            width: 100%; 
            padding: 10px; 
            font-size: 16px;
        }
        .actions { display: flex; gap: 10px; }
        .actions button { 
            padding: 8px 12px; 
            cursor: pointer; 
            background-color: #007bff; 
            color: white; 
            border: none; 
            border-radius: 5px;
            transition: 0.3s;
        }
        .actions button:hover { background-color: #0056b3; }
        h1, h2, h3 { line-height: 1.6; }
        p { line-height: 1.8; }
    </style>
</head>
<body>
    <div class="container">
        <h1>Rosh Office - بيع الموبايلات والأجهزة بالتقسيط</h1>
        <p>للتواصل: <a href="tel:07514591444">07514591444</a> | <a href="tel:07514209599">07514209599</a></p>
        <p>شروط التقسيط: <span id="terms"></span></p>
        <input type="text" id="search" class="search-bar" placeholder="ابحث عن جهاز..." onkeyup="searchProduct()">
        <h2>الأجهزة المتاحة</h2>
        <div id="products"></div>
        <button onclick="openFileInput()">إضافة جهاز جديد</button>
        <input type="file" id="imageInput" accept="image/*" class="hidden" onchange="handleImageUpload(event)">
        <h3>عدد الزوار: <span id="visitorCount"></span></h3>
    </div>

    <script>
        let visitorCount = localStorage.getItem('visitorCount') || 0;
        visitorCount++;
        localStorage.setItem('visitorCount', visitorCount);
        document.getElementById('visitorCount').textContent = visitorCount;

        let products = JSON.parse(localStorage.getItem('products')) || [];
        renderProducts();

        function openFileInput() {
            document.getElementById('imageInput').click();
        }

        function handleImageUpload(event) {
            let file = event.target.files[0];
            if (!file) return;
            
            let reader = new FileReader();
            reader.onload = function(e) {
                let productImage = e.target.result;
                addProduct(productImage);
            };
            reader.readAsDataURL(file);
        }

        function addProduct(productImage) {
            let productName = prompt("أدخل اسم الجهاز:");
            if (!productName) return;
            let installmentCount = prompt("عدد الأقساط:");
            let installmentPrice = prompt("سعر التقسيط:");
            let monthlyPayment = prompt("القسط الشهري:");
            
            let product = { productName, installmentCount, installmentPrice, monthlyPayment, productImage };
            products.push(product);
            localStorage.setItem('products', JSON.stringify(products));
            renderProducts();
        }

        function renderProducts() {
            let productsContainer = document.getElementById('products');
            productsContainer.innerHTML = '';
            products.forEach((product, index) => {
                let productDiv = document.createElement('div');
                productDiv.classList.add('product');
                productDiv.innerHTML = `<img src="${product.productImage}" alt="صورة الجهاز">
                                        <div>
                                            <h3>${product.productName}</h3>
                                            <p>عدد الأقساط: ${product.installmentCount}</p>
                                            <p>سعر التقسيط: ${product.installmentPrice}</p>
                                            <p>القسط الشهري: ${product.monthlyPayment}</p>
                                            <a href="https://wa.me/07514591444?text=استفسار عن ${product.productName}" target="_blank">تواصل عبر واتساب</a>
                                        </div>
                                        <div class="actions">
                                            <button onclick="editProduct(${index})">تعديل</button>
                                            <button onclick="deleteProduct(${index})">حذف</button>
                                        </div>`;
                productsContainer.appendChild(productDiv);
            });
        }

        function editProduct(index) {
            let product = products[index];
            product.productName = prompt("تعديل اسم الجهاز:", product.productName) || product.productName;
            product.installmentCount = prompt("تعديل عدد الأقساط:", product.installmentCount) || product.installmentCount;
            product.installmentPrice = prompt("تعديل سعر التقسيط:", product.installmentPrice) || product.installmentPrice;
            product.monthlyPayment = prompt("تعديل القسط الشهري:", product.monthlyPayment) || product.monthlyPayment;
            localStorage.setItem('products', JSON.stringify(products));
            renderProducts();
        }

        function deleteProduct(index) {
            products.splice(index, 1);
            localStorage.setItem('products', JSON.stringify(products));
            renderProducts();
        }

        function searchProduct() {
            let input = document.getElementById('search').value.toLowerCase();
            let productDivs = document.getElementsByClassName('product');
            
            for (let i = 0; i < productDivs.length; i++) {
                let productName = productDivs[i].getElementsByTagName('h3')[0].innerText.toLowerCase();
                productDivs[i].style.display = productName.includes(input) ? "flex" : "none";
            }
        }
    </script>
</body>
</html>
