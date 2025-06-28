<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>نظام إدارة العملاء</title>
    <style>
        /* Reset and Base Styles */
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Arial', sans-serif;
        }
        
        body {
            background-color: #f5f5f5;
            color: #333;
            line-height: 1.6;
        }
        
        .container {
            max-width: 800px;
            margin: 0 auto;
            padding: 20px;
        }
        
        /* Header Styles */
        header {
            background-color: #2c3e50;
            color: white;
            padding: 20px 0;
            text-align: center;
            box-shadow: 0 2px 5px rgba(0,0,0,0.1);
        }
        
        .logo {
            font-size: 28px;
            font-weight: bold;
        }
        
        /* Main Content */
        .main-content {
            margin-top: 30px;
            background-color: white;
            padding: 30px;
            border-radius: 8px;
            box-shadow: 0 2px 10px rgba(0,0,0,0.1);
        }
        
        .page-title {
            font-size: 24px;
            margin-bottom: 20px;
            color: #2c3e50;
            border-bottom: 2px solid #eee;
            padding-bottom: 10px;
        }
        
        /* Form Styles */
        .form-group {
            margin-bottom: 20px;
        }
        
        .form-group label {
            display: block;
            margin-bottom: 8px;
            font-weight: bold;
            color: #555;
        }
        
        .form-group input, .form-group select {
            width: 100%;
            padding: 12px;
            border: 1px solid #ddd;
            border-radius: 4px;
            font-size: 16px;
        }
        
        .btn {
            background-color: #3498db;
            color: white;
            border: none;
            padding: 12px 20px;
            border-radius: 4px;
            cursor: pointer;
            font-size: 16px;
            transition: background-color 0.3s;
        }
        
        .btn:hover {
            background-color: #2980b9;
        }
        
        .btn-block {
            display: block;
            width: 100%;
        }
        
        /* Result Styles */
        .result-container {
            margin-top: 30px;
            padding: 20px;
            border: 1px solid #ddd;
            border-radius: 4px;
            background-color: #f9f9f9;
        }
        
        .client-info {
            margin-bottom: 15px;
        }
        
        .client-info h3 {
            color: #2c3e50;
            margin-bottom: 10px;
        }
        
        .client-info p {
            margin-bottom: 8px;
        }
        
        /* Tabs */
        .tabs {
            display: flex;
            margin-bottom: 20px;
            border-bottom: 1px solid #ddd;
        }
        
        .tab {
            padding: 10px 20px;
            cursor: pointer;
            background-color: #f1f1f1;
            margin-left: 5px;
            border-radius: 4px 4px 0 0;
        }
        
        .tab.active {
            background-color: #3498db;
            color: white;
        }
        
        /* Responsive */
        @media (max-width: 768px) {
            .container {
                padding: 10px;
            }
            
            .main-content {
                padding: 20px;
            }
        }
    </style>
</head>
<body>
    <header>
        <div class="container">
            <div class="logo">نظام إدارة العملاء</div>
        </div>
    </header>
    
    <div class="container">
        <div class="tabs">
            <div class="tab active" onclick="showTab('register')">تسجيل عميل جديد</div>
            <div class="tab" onclick="showTab('search')">البحث عن عميل</div>
        </div>
        
        <div class="main-content">
            <!-- تسجيل عميل جديد -->
            <div id="register-tab">
                <h2 class="page-title">تسجيل عميل جديد</h2>
                
                <form id="client-form">
                    <div class="form-group">
                        <label for="client-id">الرقم التعريفي (6 أرقام)</label>
                        <input type="text" id="client-id" placeholder="أدخل الرقم التعريفي المكون من 6 أرقام" maxlength="6" required>
                    </div>
                    
                    <div class="form-group">
                        <label for="client-name">الاسم الأول</label>
                        <input type="text" id="client-name" placeholder="أدخل الاسم الأول" required>
                    </div>
                    
                    <div class="form-group">
                        <label for="client-lastname">النسبة (اسم العائلة)</label>
                        <input type="text" id="client-lastname" placeholder="أدخل النسبة" required>
                    </div>
                    
                    <div class="form-group">
                        <label for="client-phone">رقم الهاتف</label>
                        <input type="tel" id="client-phone" placeholder="أدخل رقم الهاتف" required>
                    </div>
                    
                    <button type="submit" class="btn btn-block">حفظ البيانات</button>
                </form>
                
                <div id="register-result" class="result-container" style="display: none;">
                    <h3>تم التسجيل بنجاح</h3>
                    <div id="register-client-info" class="client-info"></div>
                </div>
            </div>
            
            <!-- البحث عن عميل -->
            <div id="search-tab" style="display: none;">
                <h2 class="page-title">البحث عن عميل</h2>
                
                <div class="form-group">
                    <label for="search-id">الرقم التعريفي</label>
                    <input type="text" id="search-id" placeholder="أدخل الرقم التعريفي للبحث">
                </div>
                
                <button onclick="searchClient()" class="btn btn-block">بحث</button>
                
                <div id="search-result" class="result-container" style="display: none;">
                    <div id="client-info" class="client-info"></div>
                </div>
            </div>
        </div>
    </div>

    <script>
        // عرض تبويب معين وإخفاء الآخر
        function showTab(tabName) {
            document.querySelectorAll('.tab').forEach(tab => {
                tab.classList.remove('active');
            });
            
            document.querySelector(`.tab[onclick="showTab('${tabName}')"]`).classList.add('active');
            
            document.getElementById('register-tab').style.display = 'none';
            document.getElementById('search-tab').style.display = 'none';
            
            document.getElementById(`${tabName}-tab`).style.display = 'block';
        }
        
        // تحميل قاعدة بيانات العملاء من localStorage
        function loadClients() {
            const clients = localStorage.getItem('clients');
            return clients ? JSON.parse(clients) : {};
        }
        
        // حفظ قاعدة بيانات العملاء في localStorage
        function saveClients(clients) {
            localStorage.setItem('clients', JSON.stringify(clients));
        }
        
        // تسجيل عميل جديد
        document.getElementById('client-form').addEventListener('submit', function(e) {
            e.preventDefault();
            
            const clientId = document.getElementById('client-id').value;
            const clientName = document.getElementById('client-name').value;
            const clientLastname = document.getElementById('client-lastname').value;
            const clientPhone = document.getElementById('client-phone').value;
            
            // التحقق من صحة الرقم التعريفي
            if (clientId.length !== 6 || !/^\d+$/.test(clientId)) {
                alert('الرقم التعريفي يجب أن يكون مكون من 6 أرقام فقط');
                return;
            }
            
            // تحميل قاعدة البيانات الحالية
            const clients = loadClients();
            
            // التحقق من عدم وجود الرقم مسبقاً
            if (clients[clientId]) {
                alert('هذا الرقم التعريفي مسجل مسبقاً');
                return;
            }
            
            // إنشاء سجل العميل
            const clientData = {
                id: clientId,
                name: clientName,
                lastname: clientLastname,
                phone: clientPhone,
                registrationDate: new Date().toLocaleDateString('ar-EG')
            };
            
            // حفظ العميل في قاعدة البيانات
            clients[clientId] = clientData;
            saveClients(clients);
            
            // عرض نتيجة التسجيل
            document.getElementById('register-result').style.display = 'block';
            document.getElementById('register-client-info').innerHTML = `
                <p><strong>الرقم التعريفي:</strong> ${clientId}</p>
                <p><strong>الاسم الكامل:</strong> ${clientName} ${clientLastname}</p>
                <p><strong>رقم الهاتف:</strong> ${clientPhone}</p>
                <p><strong>تاريخ التسجيل:</strong> ${clientData.registrationDate}</p>
            `;
            
            // تفريغ الحقول
            document.getElementById('client-form').reset();
        });
        
        // البحث عن عميل
        function searchClient() {
            const clientId = document.getElementById('search-id').value;
            
            if (clientId.length !== 6 || !/^\d+$/.test(clientId)) {
                alert('الرجاء إدخال رقم تعريفي صحيح مكون من 6 أرقام');
                return;
            }
            
            const clients = loadClients();
            const client = clients[clientId];
            
            const resultContainer = document.getElementById('search-result');
            const clientInfo = document.getElementById('client-info');
            
            if (client) {
                clientInfo.innerHTML = `
                    <h3>معلومات العميل</h3>
                    <p><strong>الرقم التعريفي:</strong> ${client.id}</p>
                    <p><strong>الاسم الكامل:</strong> ${client.name} ${client.lastname}</p>
                    <p><strong>رقم الهاتف:</strong> ${client.phone}</p>
                    <p><strong>تاريخ التسجيل:</strong> ${client.registrationDate}</p>
                `;
                resultContainer.style.display = 'block';
            } else {
                clientInfo.innerHTML = '<p>لا يوجد عميل مسجل بهذا الرقم التعريفي</p>';
                resultContainer.style.display = 'block';
            }
        }
        
        // تهيئة الصفحة عند التحميل
        window.onload = function() {
            // يمكنك إضافة أي تهيئة إضافية هنا إذا لزم الأمر
        };
    </script>
</body>
</html>
