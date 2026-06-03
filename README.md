# <!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Dompet Digital - eWallet</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            padding: 20px;
        }

        .container {
            background: white;
            border-radius: 20px;
            box-shadow: 0 20px 40px rgba(0, 0, 0, 0.1);
            width: 100%;
            max-width: 450px;
            overflow: hidden;
        }

        .header {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
            padding: 30px;
            text-align: center;
        }

        .header h1 {
            font-size: 28px;
            margin-bottom: 10px;
            font-weight: 600;
        }

        .header p {
            font-size: 14px;
            opacity: 0.9;
        }

        .balance-section {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
            padding: 20px 30px 30px;
            text-align: center;
        }

        .balance-label {
            font-size: 14px;
            opacity: 0.9;
            margin-bottom: 8px;
            text-transform: uppercase;
            letter-spacing: 1px;
        }

        .balance-amount {
            font-size: 48px;
            font-weight: 700;
            margin-bottom: 10px;
        }

        .balance-amount span {
            font-size: 24px;
            margin-right: 5px;
        }

        .account-info {
            font-size: 12px;
            opacity: 0.8;
        }

        .main-content {
            padding: 30px;
        }

        .action-buttons {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 15px;
            margin-bottom: 30px;
        }

        .btn {
            padding: 15px;
            border: none;
            border-radius: 10px;
            font-size: 16px;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.3s ease;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 8px;
        }

        .btn:active {
            transform: scale(0.98);
        }

        .btn-deposit {
            background: linear-gradient(135deg, #4CAF50, #45a049);
            color: white;
        }

        .btn-deposit:hover {
            background: linear-gradient(135deg, #45a049, #3d8b40);
            box-shadow: 0 5px 15px rgba(76, 175, 80, 0.3);
        }

        .btn-withdraw {
            background: linear-gradient(135deg, #f44336, #da190b);
            color: white;
        }

        .btn-withdraw:hover {
            background: linear-gradient(135deg, #da190b, #c62828);
            box-shadow: 0 5px 15px rgba(244, 67, 54, 0.3);
        }

        .form-section {
            background: #f8f9fa;
            border-radius: 10px;
            padding: 20px;
            margin-bottom: 20px;
            display: none;
        }

        .form-section.active {
            display: block;
            animation: slideDown 0.3s ease;
        }

        @keyframes slideDown {
            from {
                opacity: 0;
                transform: translateY(-10px);
            }
            to {
                opacity: 1;
                transform: translateY(0);
            }
        }

        .form-group {
            margin-bottom: 15px;
        }

        .form-group label {
            display: block;
            margin-bottom: 5px;
            color: #333;
            font-weight: 500;
            font-size: 14px;
        }

        .form-group input,
        .form-group textarea {
            width: 100%;
            padding: 12px;
            border: 2px solid #e1e5e9;
            border-radius: 8px;
            font-size: 14px;
            transition: border-color 0.3s ease;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }

        .form-group input:focus,
        .form-group textarea:focus {
            outline: none;
            border-color: #667eea;
        }

        .form-group textarea {
            resize: vertical;
            min-height: 60px;
        }

        .btn-submit {
            width: 100%;
            padding: 12px;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
            border: none;
            border-radius: 8px;
            font-size: 16px;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.3s ease;
        }

        .btn-submit:hover {
            opacity: 0.9;
            transform: translateY(-2px);
            box-shadow: 0 5px 15px rgba(102, 126, 234, 0.3);
        }

        .btn-submit:disabled {
            background: #ccc;
            cursor: not-allowed;
            transform: none;
        }

        .transaction-history {
            margin-top: 20px;
        }

        .transaction-history h3 {
            font-size: 18px;
            color: #333;
            margin-bottom: 15px;
            padding-bottom: 10px;
            border-bottom: 2px solid #e1e5e9;
        }

        .transaction-list {
            max-height: 300px;
            overflow-y: auto;
        }

        .transaction-item {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 15px;
            border-bottom: 1px solid #f0f0f0;
            transition: background-color 0.3s ease;
        }

        .transaction-item:hover {
            background-color: #f8f9fa;
        }

        .transaction-info {
            flex: 1;
        }

        .transaction-type {
            font-weight: 600;
            font-size: 14px;
            margin-bottom: 3px;
        }

        .transaction-type.deposit {
            color: #4CAF50;
        }

        .transaction-type.withdraw {
            color: #f44336;
        }

        .transaction-desc {
            font-size: 12px;
            color: #666;
        }

        .transaction-date {
            font-size: 11px;
            color: #999;
            margin-top: 3px;
        }

        .transaction-amount {
            font-weight: 700;
            font-size: 16px;
        }

        .transaction-amount.deposit {
            color: #4CAF50;
        }

        .transaction-amount.withdraw {
            color: #f44336;
        }

        .empty-state {
            text-align: center;
            padding: 30px;
            color: #999;
        }

        .empty-state i {
            font-size: 40px;
            margin-bottom: 10px;
            display: block;
        }

        .notification {
            position: fixed;
            top: 20px;
            right: 20px;
            background: white;
            padding: 15px 20px;
            border-radius: 8px;
            box-shadow: 0 5px 20px rgba(0, 0, 0, 0.2);
            display: none;
            animation: slideInRight 0.3s ease;
            z-index: 1000;
            max-width: 300px;
        }

        .notification.success {
            border-left: 4px solid #4CAF50;
        }

        .notification.error {
            border-left: 4px solid #f44336;
        }

        @keyframes slideInRight {
            from {
                transform: translateX(100%);
                opacity: 0;
            }
            to {
                transform: translateX(0);
                opacity: 1;
            }
        }

        .notification-message {
            font-size: 14px;
            color: #333;
        }

        @media (max-width: 480px) {
            .container {
                margin: 10px;
            }
            
            .balance-amount {
                font-size: 36px;
            }
            
            .action-buttons {
                grid-template-columns: 1fr;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="header">
            <h1>💰 eWallet</h1>
            <p>Dompet Digital Anda</p>
        </div>

        <div class="balance-section">
            <div class="balance-label">Saldo Anda</div>
            <div class="balance-amount" id="balanceDisplay">
                <span>Rp</span>0
            </div>
            <div class="account-info" id="lastTransaction">
                Belum ada transaksi
            </div>
        </div>

        <div class="main-content">
            <div class="action-buttons">
                <button class="btn btn-deposit" onclick="showForm('deposit')">
                    ➕ Isi Saldo
                </button>
                <button class="btn btn-withdraw" onclick="showForm('withdraw')">
                    ➖ Tarik Dana
                </button>
            </div>

            <!-- Form Deposit -->
            <div class="form-section" id="depositForm">
                <h3 style="margin-bottom: 15px; color: #4CAF50;">Isi Saldo</h3>
                <div class="form-group">
                    <label for="depositAmount">Jumlah (Rp)</label>
                    <input type="number" id="depositAmount" placeholder="Masukkan jumlah" min="1000" step="1000">
                </div>
                <div class="form-group">
                    <label for="depositDesc">Catatan (Opsional)</label>
                    <textarea id="depositDesc" placeholder="Contoh: Transfer dari Bank BCA"></textarea>
                </div>
                <button class="btn-submit" onclick="processDeposit()">Isi Saldo</button>
            </div>

            <!-- Form Withdraw -->
            <div class="form-section" id="withdrawForm">
                <h3 style="margin-bottom: 15px; color: #f44336;">Tarik Dana</h3>
                <div class="form-group">
                    <label for="withdrawAmount">Jumlah (Rp)</label>
                    <input type="number" id="withdrawAmount" placeholder="Masukkan jumlah" min="1000" step="1000">
                </div>
                <div class="form-group">
                    <label for="withdrawDesc">Catatan (Opsional)</label>
                    <textarea id="withdrawDesc" placeholder="Contoh: Transfer ke Rekening"></textarea>
                </div>
                <button class="btn-submit" onclick="processWithdraw()">Tarik Dana</button>
            </div>

            <!-- Transaction History -->
            <div class="transaction-history">
                <h3>📋 Riwayat Transaksi</h3>
                <div class="transaction-list" id="transactionList">
                    <div class="empty-state">
                        <span>📭</span>
                        <p>Belum ada transaksi</p>
                    </div>
                </div>
            </div>
        </div>
    </div>

    <!-- Notification -->
    <div class="notification" id="notification">
        <div class="notification-message" id="notificationMessage"></div>
    </div>

    <script>
        // Initialize data
        let balance = 0;
        let transactions = [];
        
        // Load data from localStorage
        function loadData() {
            const savedBalance = localStorage.getItem('ewallet_balance');
            const savedTransactions = localStorage.getItem('ewallet_transactions');
            
            if (savedBalance) {
                balance = parseFloat(savedBalance);
            }
            
            if (savedTransactions) {
                transactions = JSON.parse(savedTransactions);
            }
            
            updateDisplay();
        }
        
        // Save data to localStorage
        function saveData() {
            localStorage.setItem('ewallet_balance', balance);
            localStorage.setItem('ewallet_transactions', JSON.stringify(transactions));
        }
        
        // Update display
        function updateDisplay() {
            // Update balance
            document.getElementById('balanceDisplay').innerHTML = 
                `<span>Rp</span>${formatNumber(balance)}`;
            
            // Update last transaction info
            if (transactions.length > 0) {
                const lastTrans = transactions[0];
                document.getElementById('lastTransaction').textContent = 
                    `Transaksi terakhir: ${lastTrans.type === 'deposit' ? 'Isi Saldo' : 'Tarik Dana'}`;
            } else {
                document.getElementById('lastTransaction').textContent = 'Belum ada transaksi';
            }
            
            // Update transaction list
            updateTransactionList();
        }
        
        // Format number to Indonesian currency format
        function formatNumber(num) {
            return num.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ".");
        }
        
        // Show notification
        function showNotification(message, type = 'success') {
            const notification = document.getElementById('notification');
            const notificationMessage = document.getElementById('notificationMessage');
            
            notification.className = `notification ${type}`;
            notificationMessage.textContent = message;
            notification.style.display = 'block';
            
            setTimeout(() => {
                notification.style.display = 'none';
            }, 3000);
        }
        
        // Show form
        function showForm(type) {
            const depositForm = document.getElementById('depositForm');
            const withdrawForm = document.getElementById('withdrawForm');
            
            if (type === 'deposit') {
                depositForm.classList.toggle('active');
                withdrawForm.classList.remove('active');
                document.getElementById('depositAmount').focus();
            } else {
                withdrawForm.classList.toggle('active');
                depositForm.classList.remove('active');
                document.getElementById('withdrawAmount').focus();
            }
        }
        
        // Process deposit
        function processDeposit() {
            const amountInput = document.getElementById('depositAmount');
            const descInput = document.getElementById('depositDesc');
            const amount = parseInt(amountInput.value);
            
            // Validation
            if (!amount || amount < 1000) {
                showNotification('❌ Minimal isi saldo Rp 1.000', 'error');
                return;
            }
            
            if (amount > 100000000) {
                showNotification('❌ Maksimal isi saldo Rp 100.000.000', 'error');
                return;
            }
            
            // Process transaction
            balance += amount;
            
            const transaction = {
                id: Date.now(),
                type: 'deposit',
                amount: amount,
                description: descInput.value || 'Isi saldo',
                date: new Date().toLocaleString('id-ID')
            };
            
            transactions.unshift(transaction);
            
            // Save and update display
            saveData();
            updateDisplay();
            
            // Clear form
            amountInput.value = '';
            descInput.value = '';
            document.getElementById('depositForm').classList.remove('active');
            
            // Show notification
            showNotification(`✅ Berhasil isi saldo Rp ${formatNumber(amount)}`);
        }
        
        // Process withdraw
        function processWithdraw() {
            const amountInput = document.getElementById('withdrawAmount');
            const descInput = document.getElementById('withdrawDesc');
            const amount = parseInt(amountInput.value);
            
            // Validation
            if (!amount || amount < 1000) {
                showNotification('❌ Minimal tarik dana Rp 1.000', 'error');
                return;
            }
            
            if (amount > balance) {
                showNotification('❌ Saldo tidak mencukupi', 'error');
                return;
            }
            
            // Process transaction
            balance -= amount;
            
            const transaction = {
                id: Date.now(),
                type: 'withdraw',
                amount: amount,
                description: descInput.value || 'Tarik dana',
                date: new Date().toLocaleString('id-ID')
            };
            
            transactions.unshift(transaction);
            
            // Save and update display
            saveData();
            updateDisplay();
            
            // Clear form
            amountInput.value = '';
            descInput.value = '';
            document.getElementById('withdrawForm').classList.remove('active');
            
            // Show notification
            showNotification(`✅ Berhasil tarik dana Rp ${formatNumber(amount)}`);
        }
        
        // Update transaction list
        function updateTransactionList() {
            const listContainer = document.getElementById('transactionList');
            
            if (transactions.length === 0) {
                listContainer.innerHTML = `
                    <div class="empty-state">
                        <span>📭</span>
                        <p>Belum ada transaksi</p>
                    </div>
                `;
                return;
            }
            
            listContainer.innerHTML = transactions.map(trans => `
                <div class="transaction-item">
                    <div class="transaction-info">
                        <div class="transaction-type ${trans.type}">
                            ${trans.type === 'deposit' ? '📥 Isi Saldo' : '📤 Tarik Dana'}
                        </div>
                        <div class="transaction-desc">${trans.description}</div>
                        <div class="transaction-date">${trans.date}</div>
                    </div>
                    <div class="transaction-amount ${trans.type}">
                        ${trans.type === 'deposit' ? '+' : '-'} Rp ${formatNumber(trans.amount)}
                    </div>
                </div>
            `).join('');
        }
        
        // Handle Enter key for forms
        document.addEventListener('keypress', function(e) {
            if (e.key === 'Enter') {
                const depositForm = document.getElementById('depositForm');
                const withdrawForm = document.getElementById('withdrawForm');
                
                if (depositForm.classList.contains('active')) {
                    processDeposit();
                } else if (withdrawForm.classList.contains('active')) {
                    processWithdraw();
                }
            }
        });
        
        // Close forms when clicking outside
        document.addEventListener('click', function(e) {
            const depositForm = document.getElementById('depositForm');
            const withdrawForm = document.getElementById('withdrawForm');
            
            if (!e.target.closest('.form-section') && 
                !e.target.closest('.btn-deposit') && 
                !e.target.closest('.btn-withdraw')) {
                depositForm.classList.remove('active');
                withdrawForm.classList.remove('active');
            }
        });
        
        // Initialize app
        loadData();
    </script>
</body>
</html>
