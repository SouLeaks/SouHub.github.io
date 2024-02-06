<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Shopping Store with PayPal</title>
    <script src="https://www.paypal.com/sdk/js?client-id=YOUR_CLIENT_ID&currency=USD"></script>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 0;
            padding: 0;
            background-color: #f0f0f0;
        }
        header {
            background-color: #333;
            color: #fff;
            padding: 10px;
            text-align: center;
        }
        .container {
            max-width: 800px;
            margin: 20px auto;
            padding: 0 20px;
        }
        .product {
            border: 1px solid #ccc;
            margin-bottom: 20px;
            padding: 10px;
            background-color: #fff;
        }
        .product h3 {
            margin-top: 0;
        }
        .cart {
            border: 1px solid #ccc;
            padding: 10px;
            background-color: #fff;
        }
        .cart h2 {
            margin-top: 0;
        }
        #paypal-button-container {
            text-align: center;
        }
    </style>
</head>
<body>
    <header>
        <h1>Shopping Store with PayPal</h1>
    </header>
    <div class="container">
        <div id="product-list"></div>
        <div class="cart">
            <h2>Cart</h2>
            <div id="cart-items"></div>
            <div id="paypal-button-container"></div>
        </div>
    </div>

    <script>
        // Product data
        const products = [
            { id: 1, name: "Product 1", price: 10 },
            { id: 2, name: "Product 2", price: 20 },
            { id: 3, name: "Product 3", price: 30 }
        ];

        // Display products
        const productList = document.getElementById("product-list");
        products.forEach(product => {
            productList.innerHTML += `
                <div class="product">
                    <h3>${product.name}</h3>
                    <p>Price: $${product.price}</p>
                    <button onclick="addToCart(${product.id}, '${product.name}', ${product.price})">Add to Cart</button>
                </div>
            `;
        });

        // Cart items
        let cartItems = [];

        // Function to add item to cart
        function addToCart(id, name, price) {
            cartItems.push({ id, name, price });
            displayCart();
        }

        // Function to display cart
        function displayCart() {
            const cartItemsDiv = document.getElementById("cart-items");
            cartItemsDiv.innerHTML = "";
            let total = 0;
            cartItems.forEach(item => {
                cartItemsDiv.innerHTML += `
                    <p>${item.name} - $${item.price}</p>
                `;
                total += item.price;
            });
            cartItemsDiv.innerHTML += `<p>Total: $${total}</p>`;
        }

        // PayPal button
        paypal.Buttons({
            createOrder: function(data, actions) {
                return actions.order.create({
                    purchase_units: [{
                        amount: {
                            value: '1.00'
                        }
                    }]
                });
            },
            onApprove: function(data, actions) {
                return actions.order.capture().then(function(details) {
                    alert('Transaction completed by ' + details.payer.name.given_name);
                });
            }
        }).render('#paypal-button-container');
    </script>
</body>
</html>
