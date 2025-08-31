<!DOCTYPE html>
<html lang="ku" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>سیستەمی خاوەن کار - خواردنگە</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        :root {
            --primary: #2c3e50;
            --secondary: #3498db;
            --accent: #e74c3c;
            --light: #ecf0f1;
            --dark: #2c3e50;
            --success: #27ae60;
            --warning: #f39c12;
            --danger: #c0392b;
        }
        
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }
        
        body {
            background: linear-gradient(to right, var(--primary), var(--secondary));
            color: var(--dark);
            min-height: 100vh;
            padding: 20px;
        }
        
        .container {
            max-width: 1200px;
            margin: 0 auto;
            background: rgba(255, 255, 255, 0.95);
            border-radius: 15px;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.15);
            overflow: hidden;
        }
        
        header {
            background: linear-gradient(to right, var(--primary), var(--secondary));
            color: white;
            padding: 20px;
            text-align: center;
        }
        
        .card {
            background: white;
            border-radius: 10px;
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.1);
            margin-bottom: 20px;
            overflow: hidden;
        }
        
        .card-header {
            background: var(--primary);
            color: white;
            padding: 15px;
            font-weight: bold;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }
        
        .card-body {
            padding: 20px;
        }
        
        .form-group {
            margin-bottom: 15px;
        }
        
        label {
            display: block;
            margin-bottom: 5px;
            font-weight: bold;
        }
        
        input, select, textarea {
            width: 100%;
            padding: 12px;
            border: 1px solid #ddd;
            border-radius: 5px;
            font-size: 16px;
        }
        
        button {
            background: var(--primary);
            color: white;
            border: none;
            padding: 12px 20px;
            border-radius: 5px;
            cursor: pointer;
            font-size: 16px;
            font-weight: bold;
            transition: all 0.3s;
        }
        
        button:hover {
            background: var(--secondary);
            transform: translateY(-2px);
        }
        
        .btn-success {
            background: var(--success);
        }
        
        .btn-danger {
            background: var(--danger);
        }
        
        .btn-warning {
            background: var(--warning);
            color: var(--dark);
        }
        
        .order-list {
            list-style: none;
            margin-top: 20px;
        }
        
        .order-item {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 10px 0;
            border-bottom: 1px solid #eee;
        }
        
        .history-item {
            padding: 15px;
            border-bottom: 1px solid #eee;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }
        
        .notification {
            position: fixed;
            top: 20px;
            right: 20px;
            padding: 15px 20px;
            background: var(--success);
            color: white;
            border-radius: 5px;
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.2);
            transform: translateX(100%);
            transition: transform 0.3s;
            z-index: 1000;
        }
        
        .notification.show {
            transform: translateX(0);
        }
        
        @media (max-width: 768px) {
            .menu-grid {
                grid-template-columns: 1fr;
            }
        }
        
        .api-status {
            padding: 10px;
            margin: 10px 0;
            border-radius: 5px;
            text-align: center;
            font-weight: bold;
        }
        
        .api-status.connected {
            background-color: var(--success);
            color: white;
        }
        
        .api-status.disconnected {
            background-color: var(--danger);
            color: white;
        }
    </style>
</head>
<body>
    <div class="container">
        <header>
            <h1>سیستەمی خاوەن کار - خواردنگە</h1>
            <p>هۆست: owner.example.com</p>
        </header>
        
        <div class="api-status disconnected" id="api-status">
            پەیوەندی بە سیستەمی موشتەریەوە نییە
        </div>
        
        <div class="card">
            <div class="card-header">
                <span>زیادکردنی خواردنەوە</span>
            </div>
            <div class="card-body">
                <div class="form-group">
                    <label for="food-name">ناوی خواردن</label>
                    <input type="text" id="food-name" placeholder="ناوی خواردن بنووسە">
                </div>
                <div class="form-group">
                    <label for="food-price">نرخی خواردن (دینار)</label>
                    <input type="number" id="food-price" placeholder="نرخی خواردن بنووسە">
                </div>
                <div class="form-group">
                    <label for="food-image">وێنەی خواردن</label>
                    <input type="file" id="food-image">
                </div>
                <button id="add-food-btn">زیادکردنی خواردن</button>
            </div>
        </div>
        
        <div class="card">
            <div class="card-header">
                <span>داواکاریەکان</span>
            </div>
            <div class="card-body">
                <div id="orders-list">
                    <div class="order-item">
                        <div>
                            <div>مێز: ٥ - کۆدی داواکاری: #ORD123</div>
                            <div>برۆکۆلی و مریشک - ٣٥٠٠ دینار</div>
                        </div>
                        <div>
                            <button class="btn-success" onclick="completeOrder('#ORD123')">فرۆشتنی</button>
                            <button class="btn-danger">ڕەتکردنەوە</button>
                        </div>
                    </div>
                    <div class="order-item">
                        <div>
                            <div>مێز: ٢ - کۆدی داواکاری: #ORD124</div>
                            <div>پیتزا - ٤٥٠٠ دینار</div>
                        </div>
                        <div>
                            <button class="btn-success" onclick="completeOrder('#ORD124')">فرۆشتنی</button>
                            <button class="btn-danger">ڕەتکردنەوە</button>
                        </div>
                    </div>
                </div>
            </div>
        </div>
        
        <div class="card">
            <div class="card-header">
                <span>مێژووی فرۆشتن</span>
            </div>
            <div class="card-body">
                <div id="sales-history">
                    <div class="history-item">
                        <div>
                            <div>ڕێکەوت: ٢٠٢٣/١٠/٢٥ - کات: ١٣:٤٥</div>
                            <div>مێز: ٣ - کۆی گشتی: ٨٥٠٠ دینار - کۆدی داواکاری: #ORD122</div>
                        </div>
                        <button class="btn-warning">چاپکردن</button>
                    </div>
                    <div class="history-item">
                        <div>
                            <div>ڕێکەوت: ٢٠٢٣/١٠/٢٤ - کات: ١٩:٣٠</div>
                            <div>مێز: ١ - کۆی گشتی: ٦٥٠٠ دینار - کۆدی داواکاری: #ORD121</div>
                        </div>
                        <button class="btn-warning">چاپکردن</button>
                    </div>
                </div>
            </div>
        </div>
    </div>

    <div class="notification" id="notification">
        <span id="notification-text">ئەمە نامەیەکی ئاگادارکردنەوەیە</span>
    </div>

    <script>
        // نیشاندانی ئاگاداری
        function showNotification(message) {
            const notification = document.getElementById('notification');
            const notificationText = document.getElementById('notification-text');
            
            notificationText.textContent = message;
            notification.classList.add('show');
            
            setTimeout(() => {
                notification.classList.remove('show');
            }, 3000);
        }

        // زیادکردنی خواردن بۆ مێنو
        document.getElementById('add-food-btn').addEventListener('click', () => {
            const foodName = document.getElementById('food-name').value;
            const foodPrice = document.getElementById('food-price').value;
            
            if (foodName && foodPrice) {
                // لێرەدا دەبێت API call بۆ سیستەمی موشتەری بکرێت بۆ نوێکردنەوەی مێنو
                showNotification(`"${foodName}" زیادکرا بۆ مێنو و نێردرا بۆ سیستەمی موشتەری`);
                document.getElementById('food-name').value = '';
                document.getElementById('food-price').value = '';
                
                // نیشاندانی پەیوەندییەکی سەرکەوتوو
                document.getElementById('api-status').textContent = "پەیوەندی بە سیستەمی موشتەریەوە دۆزرایەوە";
                document.getElementById('api-status').className = "api-status connected";
            } else {
                showNotification('تکایە هەموو خانەکان پڕ بکەرەوە');
            }
        });

        // تەواوکردنی داواکاری
        function completeOrder(orderId) {
            // لێرەدا دەبێت API call بۆ سیستەمی موشتەری بکرێت بۆ نوێکردنەوەی دۆخ
            showNotification(`داواکاری ${orderId} تەواوکرا و نێردرا بۆ سیستەمی موشتەری`);
            
            // زیادکردنی بە مێژووی فرۆشتن
            const salesHistory = document.getElementById('sales-history');
            const newSale = document.createElement('div');
            newSale.className = 'history-item';
            newSale.innerHTML = `
                <div>
                    <div>ڕێکەوت: ${new Date().toLocaleDateString('ku')} - کات: ${new Date().toLocaleTimeString('ku')}</div>
                    <div>کۆدی داواکاری: ${orderId} - کۆی گشتی: ٨٥٠٠ دینار</div>
                </div>
                <button class="btn-warning">چاپکردن</button>
            `;
            salesHistory.prepend(newSale);
            
            // سڕینەوەی لە لیستی داواکاریەکان
            const orderItems = document.querySelectorAll('.order-item');
            orderItems.forEach(item => {
                if (item.textContent.includes(orderId)) {
                    item.remove();
                }
            });
        }

        // لێکۆڵینەوەی داواکاریە نوێیەکان لە سیستەمی موشتەری
        function checkForNewOrders() {
            // لێرەدا دەبێت API call بۆ سیستەمی موشتەری بکرێت
            // بۆ نموونە، هەر ١٠ چرکە جارێک لێکۆڵینەوە بکە
            console.log("پشکنین بۆ داواکاری نوێ...");
        }

        // دەستپێکردنی پشکنین بۆ داواکاری نوێ
        setInterval(checkForNewOrders, 10000);
    </script>
</body>
</html>
