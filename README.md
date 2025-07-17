<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Food Munch - Login | Products | Cart</title>
    <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.5.2/css/bootstrap.min.css" />
    <style>
        body {
            margin: 0;
            padding: 0;
        }

        .bg-container-1 {
            background-image: url("https://images.pexels.com/photos/1410235/pexels-photo-1410235.jpeg?auto=compress&cs=tinysrgb&w=600");
            background-size: cover;
            background-position: center;
            height: 100vh;
            width: 100vw;
        }

        .heading-1 {
            color: black;
            font-size: 40px;
            font-weight: bold;
            text-align: center;
            padding: 10px;
        }

        .product-card {
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.2);
            border-radius: 12px;
            transition: transform 0.2s;
        }

        .product-card:hover {
            transform: scale(1.03);
        }

        .product-img {
            height: 200px;
            object-fit: cover;
        }

        .navbar-brand img {
            height: 50px;
        }
    </style>
</head>

<body>
    <!-- Login Page -->
    <div id="login">
        <div class="bg-container-1 d-flex flex-column justify-content-center align-items-center">
            <h1 class="heading-1">FOOD MUNCH</h1>
            <h2 class="heading-1">Get Healthy & Delicious Food at Anytime</h2>
            <div class="card p-4" style="width: 350px;">
                <h3 class="text-center">Login</h3>
                <form onsubmit="return validateForm()">
                    <div class="form-group">
                        <label for="nameInput">Username</label>
                        <input type="text" class="form-control" id="nameInput" required />
                    </div>
                    <div class="form-group">
                        <label for="passwordInput">Password</label>
                        <input type="password" class="form-control" id="passwordInput" required />
                    </div>
                    <button type="submit" class="btn btn-primary btn-block">Submit</button>
                </form>
            </div>
        </div>
    </div>

    <!-- Product Page -->
    <div id="product" style="display: none;">
        <nav class="navbar navbar-expand-lg navbar-dark bg-dark">
            <a class="navbar-brand" href="#">
                <img src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcTonw2DXk5DYtDwco4EJhK45hi5vsDH48i2mw&s" alt="Logo">
            </a>
            <div class="collapse navbar-collapse">
                <div class="navbar-nav">
                    <a class="nav-link text-white" href="#" onclick="showSection('product')">Products</a>
                    <a class="nav-link text-white" href="#" onclick="showSection('cart')">Cart</a>
                </div>
            </div>
        </nav>
        <div class="container my-5">
            <h2 class="text-center mb-4">Our Best Dishes</h2>
            <div class="row" id="product-list"></div>
            <div class="text-center mt-4">
                <button class="btn btn-primary" onclick="showSection('cart')">Go to Cart</button>
            </div>
        </div>
    </div>

    <!-- Cart Page -->
    <div id="cart" style="display: none;">
        <nav class="navbar navbar-dark bg-dark">
            <a class="navbar-brand text-white" href="#" onclick="showSection('product')">Back to Products</a>
        </nav>
        <div class="container my-5">
            <h2 class="text-center mb-4">Your Cart</h2>
            <ul class="list-group mb-3" id="cart-items"></ul>
            <h4 class="text-right">Total: ₹<span id="total-price">0</span></h4>
            <div class="text-center">
                <button class="btn btn-success">Checkout</button>
            </div>
        </div>
    </div>

    <script>
        const products = [{
                id: 1,
                name: "Dosa",
                price: 50,
                image: "https://www.awesomecuisine.com/wp-content/uploads/2009/06/Plain-Dosa.jpg"
            },
            {
                id: 2,
                name: "Puri",
                price: 80,
                image: "https://desertfoodfeed.com/wp-content/uploads/2020/12/rava-poori-puri-recipe.jpg"
            },
            {
                id: 3,
                name: "Chapathi",
                price: 80,
                image: "https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcTxU-peE_KxbUc7IwmZbATI0soRxh71cCkHOw&s"
            }
        ];

        function showSection(sectionId) {
            document.getElementById("login").style.display = "none";
            document.getElementById("product").style.display = "none";
            document.getElementById("cart").style.display = "none";

            document.getElementById(sectionId).style.display = "block";

            if (sectionId === 'cart') {
                displayCartItems();
            }
        }

        function validateForm() {
            const username = document.getElementById("nameInput").value;
            const password = document.getElementById("passwordInput").value;

            if (username === "admin" && password === "123456") {
                showSection("product");
            } else {
                alert("Invalid credentials");
            }
            return false;
        }

        function addToCart(product) {
            let cart = JSON.parse(localStorage.getItem("cart")) || [];
            cart.push(product);
            localStorage.setItem("cart", JSON.stringify(cart));
            alert(`${product.name} added to cart!`);
        }

        function displayProducts() {
            const container = document.getElementById("product-list");
            products.forEach(product => {
                const col = document.createElement("div");
                col.className = "col-md-4 mb-4";
                col.innerHTML = `
                    <div class="card product-card">
                        <img src="${product.image}" class="card-img-top product-img" alt="${product.name}">
                        <div class="card-body">
                            <h5 class="card-title">${product.name}</h5>
                            <p class="card-text">₹${product.price}</p>
                            <button class="btn btn-success btn-block" onclick='addToCart(${JSON.stringify(product)})'>Add to Cart</button>
                        </div>
                    </div>
                `;
                container.appendChild(col);
            });
        }

        function displayCartItems() {
            const cart = JSON.parse(localStorage.getItem("cart")) || [];
            const container = document.getElementById("cart-items");
            container.innerHTML = "";
            let total = 0;

            cart.forEach(item => {
                const li = document.createElement("li");
                li.className = "list-group-item d-flex justify-content-between align-items-center";
                li.innerHTML = `${item.name} <span>₹${item.price}</span>`;
                container.appendChild(li);
                total += item.price;
            });

            document.getElementById("total-price").textContent = total;
        }

        displayProducts();
    </script>
</body>

</html>
