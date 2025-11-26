<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>FriendConnect - Komunikasi Bluetooth</title>
    <style>
        :root {
            --primary-dark: #000000;
            --primary-purple: #8A2BE2;
            --primary-blue: #00BFFF;
            --accent-purple: #9932CC;
            --accent-blue: #1E90FF;
            --text-light: #FFFFFF;
            --text-gray: #CCCCCC;
            --card-bg: rgba(30, 30, 46, 0.7);
            --card-border: rgba(138, 43, 226, 0.3);
        }

        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }

        body {
            background: linear-gradient(135deg, var(--primary-dark), #1a1a2e);
            color: var(--text-light);
            min-height: 100vh;
            overflow-x: hidden;
        }

        .container {
            max-width: 1200px;
            margin: 0 auto;
            padding: 20px;
        }

        /* Header Styles */
        header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 15px 0;
            border-bottom: 1px solid var(--card-border);
            margin-bottom: 30px;
        }

        .logo {
            font-size: 24px;
            font-weight: bold;
            background: linear-gradient(90deg, var(--primary-purple), var(--primary-blue));
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
        }

        .menu-btn {
            position: fixed;
            top: 20px;
            left: 20px;
            z-index: 1000;
            background: rgba(30, 30, 46, 0.8);
            border: none;
            border-radius: 50%;
            width: 50px;
            height: 50px;
            display: flex;
            align-items: center;
            justify-content: center;
            cursor: pointer;
            color: var(--text-light);
            font-size: 20px;
            backdrop-filter: blur(10px);
            box-shadow: 0 4px 15px rgba(0, 0, 0, 0.3);
            transition: all 0.3s ease;
        }

        .menu-btn:hover {
            transform: scale(1.1);
            background: rgba(138, 43, 226, 0.3);
        }

        .btn {
            padding: 10px 20px;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            font-weight: bold;
            transition: all 0.3s ease;
            text-decoration: none;
            display: inline-block;
            text-align: center;
        }

        .btn-primary {
            background: linear-gradient(90deg, var(--primary-purple), var(--primary-blue));
            color: var(--text-light);
        }

        .btn-primary:hover {
            transform: translateY(-2px);
            box-shadow: 0 5px 15px rgba(138, 43, 226, 0.4);
        }

        .btn-secondary {
            background: transparent;
            color: var(--text-light);
            border: 1px solid var(--primary-purple);
        }

        .btn-secondary:hover {
            background: rgba(138, 43, 226, 0.1);
        }

        /* Card Styles */
        .card {
            background: var(--card-bg);
            border-radius: 10px;
            padding: 25px;
            margin-bottom: 20px;
            box-shadow: 0 4px 15px rgba(0, 0, 0, 0.2);
            border: 1px solid var(--card-border);
            backdrop-filter: blur(10px);
        }

        .card-title {
            font-size: 20px;
            margin-bottom: 15px;
            color: var(--primary-blue);
        }

        /* Form Styles */
        .form-group {
            margin-bottom: 20px;
        }

        label {
            display: block;
            margin-bottom: 8px;
            color: var(--text-gray);
        }

        input[type="text"],
        input[type="email"],
        input[type="password"] {
            width: 100%;
            padding: 12px 15px;
            border-radius: 5px;
            border: 1px solid rgba(138, 43, 226, 0.3);
            background: rgba(30, 30, 46, 0.5);
            color: var(--text-light);
            font-size: 16px;
            transition: all 0.3s ease;
        }

        input:focus {
            outline: none;
            border-color: var(--primary-purple);
            box-shadow: 0 0 0 2px rgba(138, 43, 226, 0.2);
        }

        /* Page Styles */
        .page {
            display: none;
            animation: fadeIn 0.5s ease;
        }

        .page.active {
            display: block;
        }

        @keyframes fadeIn {
            from { opacity: 0; }
            to { opacity: 1; }
        }

        /* Login & Register Pages */
        .auth-container {
            max-width: 400px;
            margin: 50px auto;
        }

        .auth-tabs {
            display: flex;
            margin-bottom: 20px;
            border-bottom: 1px solid var(--card-border);
        }

        .auth-tab {
            flex: 1;
            padding: 10px;
            text-align: center;
            cursor: pointer;
            transition: all 0.3s ease;
        }

        .auth-tab.active {
            color: var(--primary-blue);
            border-bottom: 2px solid var(--primary-blue);
        }

        /* Main Page */
        .contacts-list {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
            gap: 15px;
        }

        .contact-item {
            background: rgba(30, 30, 46, 0.5);
            border-radius: 8px;
            padding: 15px;
            display: flex;
            align-items: center;
            cursor: pointer;
            transition: all 0.3s ease;
            border: 1px solid transparent;
        }

        .contact-item:hover {
            background: rgba(138, 43, 226, 0.1);
            border-color: var(--primary-purple);
            transform: translateY(-3px);
        }

        .contact-avatar {
            width: 40px;
            height: 40px;
            border-radius: 50%;
            background: linear-gradient(135deg, var(--primary-purple), var(--primary-blue));
            display: flex;
            align-items: center;
            justify-content: center;
            margin-right: 15px;
            font-weight: bold;
        }

        .contact-info {
            flex: 1;
        }

        .contact-name {
            font-weight: bold;
            margin-bottom: 5px;
        }

        .contact-status {
            font-size: 12px;
            color: var(--text-gray);
        }

        .status-online {
            color: #00FF00;
        }

        /* Menu Page */
        .menu-options {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
            gap: 20px;
            margin-top: 20px;
        }

        .menu-option {
            background: rgba(30, 30, 46, 0.5);
            border-radius: 8px;
            padding: 20px;
            text-align: center;
            cursor: pointer;
            transition: all 0.3s ease;
            border: 1px solid transparent;
        }

        .menu-option:hover {
            background: rgba(138, 43, 226, 0.1);
            border-color: var(--primary-purple);
            transform: translateY(-5px);
        }

        .menu-icon {
            font-size: 40px;
            margin-bottom: 10px;
            color: var(--primary-blue);
        }

        /* Bluetooth Device Page */
        .device-container {
            text-align: center;
            padding: 30px;
        }

        .device-status {
            display: inline-block;
            padding: 10px 20px;
            border-radius: 20px;
            margin: 15px 0;
            font-weight: bold;
        }

        .status-active {
            background: rgba(0, 255, 0, 0.2);
            color: #00FF00;
            border: 1px solid #00FF00;
        }

        .status-inactive {
            background: rgba(255, 0, 0, 0.2);
            color: #FF0000;
            border: 1px solid #FF0000;
        }

        .device-info {
            font-size: 18px;
            margin: 15px 0;
            color: var(--primary-blue);
        }

        .device-list {
            margin-top: 20px;
            text-align: left;
        }

        .device-item {
            background: rgba(30, 30, 46, 0.5);
            border-radius: 8px;
            padding: 15px;
            margin-bottom: 10px;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        /* Scan Page */
        .scan-container {
            text-align: center;
        }

        #camera-preview {
            width: 100%;
            max-width: 500px;
            height: 300px;
            background: rgba(30, 30, 46, 0.7);
            border-radius: 10px;
            margin: 20px auto;
            display: flex;
            align-items: center;
            justify-content: center;
            border: 2px dashed var(--primary-purple);
        }

        #manual-input {
            margin-top: 20px;
        }

        /* Chat Page */
        .chat-header {
            display: flex;
            align-items: center;
            padding-bottom: 15px;
            border-bottom: 1px solid var(--card-border);
            margin-bottom: 20px;
        }

        .back-btn {
            margin-right: 15px;
            font-size: 20px;
            cursor: pointer;
            color: var(--primary-blue);
        }

        .chat-messages {
            height: 400px;
            overflow-y: auto;
            padding: 15px;
            background: rgba(0, 0, 0, 0.2);
            border-radius: 10px;
            margin-bottom: 20px;
        }

        .message {
            margin-bottom: 15px;
            padding: 10px 15px;
            border-radius: 10px;
            max-width: 80%;
            position: relative;
        }

        .message.sent {
            background: linear-gradient(90deg, var(--primary-purple), var(--accent-purple));
            margin-left: auto;
            border-bottom-right-radius: 0;
        }

        .message.received {
            background: rgba(30, 144, 255, 0.3);
            border-bottom-left-radius: 0;
        }

        .message-time {
            font-size: 10px;
            margin-top: 5px;
            text-align: right;
            opacity: 0.7;
        }

        .chat-input {
            display: flex;
            gap: 10px;
        }

        .chat-input input {
            flex: 1;
        }

        /* Notification */
        .notification {
            position: fixed;
            top: 20px;
            right: 20px;
            background: var(--card-bg);
            border-left: 4px solid var(--primary-blue);
            padding: 15px 20px;
            border-radius: 5px;
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.3);
            z-index: 1000;
            max-width: 350px;
            display: none;
        }

        .notification.show {
            display: block;
            animation: slideIn 0.5s ease;
        }

        @keyframes slideIn {
            from { transform: translateX(100%); opacity: 0; }
            to { transform: translateX(0); opacity: 1; }
        }

        .notification-title {
            font-weight: bold;
            margin-bottom: 5px;
            color: var(--primary-blue);
        }

        .notification-actions {
            margin-top: 10px;
            display: flex;
            gap: 10px;
        }

        /* Bluetooth Devices Page */
        .devices-container {
            text-align: center;
        }

        .devices-list {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
            gap: 15px;
            margin-top: 20px;
        }

        .device-card {
            background: rgba(30, 30, 46, 0.5);
            border-radius: 8px;
            padding: 20px;
            text-align: center;
            transition: all 0.3s ease;
            border: 1px solid transparent;
        }

        .device-card:hover {
            background: rgba(138, 43, 226, 0.1);
            border-color: var(--primary-purple);
            transform: translateY(-3px);
        }

        .device-name {
            font-weight: bold;
            margin-bottom: 10px;
            color: var(--primary-blue);
        }

        .device-id {
            font-size: 14px;
            color: var(--text-gray);
            margin-bottom: 15px;
        }

        .device-actions {
            display: flex;
            gap: 10px;
            justify-content: center;
        }
    </style>
</head>
<body>
    <!-- Menu Button -->
    <button class="menu-btn" id="menu-toggle">☰</button>

    <div class="container">
        <!-- Header -->
        <header>
            <div class="logo">FriendConnect</div>
            <div id="header-buttons">
                <!-- Buttons will be dynamically added based on page -->
            </div>
        </header>

        <!-- Login/Register Page -->
        <div id="auth-page" class="page active">
            <div class="auth-container">
                <div class="card">
                    <div class="auth-tabs">
                        <div class="auth-tab active" id="login-tab">Login</div>
                        <div class="auth-tab" id="register-tab">Daftar</div>
                    </div>
                    
                    <div id="login-form">
                        <div class="form-group">
                            <label for="login-email">Email</label>
                            <input type="email" id="login-email" placeholder="Masukkan email Anda">
                        </div>
                        <div class="form-group">
                            <label for="login-password">Password</label>
                            <input type="password" id="login-password" placeholder="Masukkan password Anda">
                        </div>
                        <button class="btn btn-primary" id="login-btn">Login</button>
                    </div>
                    
                    <div id="register-form" style="display: none;">
                        <div class="form-group">
                            <label for="register-email">Email</label>
                            <input type="email" id="register-email" placeholder="Masukkan email Anda">
                        </div>
                        <div class="form-group">
                            <label for="register-password">Password</label>
                            <input type="password" id="register-password" placeholder="Buat password">
                        </div>
                        <div class="form-group">
                            <label for="confirm-password">Konfirmasi Password</label>
                            <input type="password" id="confirm-password" placeholder="Konfirmasi password">
                        </div>
                        <button class="btn btn-primary" id="register-btn">Daftar</button>
                    </div>
                </div>
            </div>
        </div>

        <!-- Main Page (After Login) -->
        <div id="main-page" class="page">
            <div class="card">
                <h2 class="card-title">Kontak Saya</h2>
                <div class="contacts-list" id="contacts-list">
                    <!-- Contacts will be dynamically added here -->
                </div>
            </div>
        </div>

        <!-- Menu Page -->
        <div id="menu-page" class="page">
            <div class="card">
                <h2 class="card-title">Menu</h2>
                <div class="menu-options">
                    <div class="menu-option" id="host-bluetooth-btn">
                        <div class="menu-icon">📡</div>
                        <div>Host Bluetooth Device</div>
                    </div>
                    <div class="menu-option" id="scan-bluetooth-btn">
                        <div class="menu-icon">🔍</div>
                        <div>Scan Bluetooth Devices</div>
                    </div>
                    <div class="menu-option" id="enable-bluetooth-btn">
                        <div class="menu-icon">🔗</div>
                        <div>Aktifkan Bluetooth</div>
                    </div>
                </div>
            </div>
        </div>

        <!-- Host Bluetooth Device Page -->
        <div id="host-device-page" class="page">
            <div class="card">
                <h2 class="card-title">Host Bluetooth Device</h2>
                <div class="device-container">
                    <p>Buat device Bluetooth Anda sendiri untuk dihubungi oleh teman:</p>
                    <div class="device-status" id="device-status">Tidak Aktif</div>
                    <div class="device-info">Nama Device: <span id="device-name-display"></span></div>
                    <div class="device-info">ID Device: <span id="device-id-display"></span></div>
                    
                    <div class="device-actions" style="margin-top: 20px;">
                        <button class="btn btn-primary" id="start-hosting-btn">Mulai Hosting</button>
                        <button class="btn btn-secondary" id="stop-hosting-btn">Stop Hosting</button>
                    </div>
                    
                    <div class="device-list" id="connected-devices">
                        <h3>Perangkat Terhubung:</h3>
                        <!-- Connected devices will be listed here -->
                    </div>
                </div>
            </div>
        </div>

        <!-- Scan Bluetooth Devices Page -->
        <div id="scan-devices-page" class="page">
            <div class="card">
                <h2 class="card-title">Scan Bluetooth Devices</h2>
                <div class="devices-container">
                    <p>Memindai perangkat Bluetooth di sekitar...</p>
                    <div class="device-actions" style="margin-bottom: 20px;">
                        <button class="btn btn-primary" id="start-scan-btn">Mulai Scan</button>
                        <button class="btn btn-secondary" id="stop-scan-btn">Stop Scan</button>
                    </div>
                    
                    <div class="devices-list" id="devices-list">
                        <!-- Available devices will be listed here -->
                    </div>
                </div>
            </div>
        </div>

        <!-- Chat Page -->
        <div id="chat-page" class="page">
            <div class="card">
                <div class="chat-header">
                    <div class="back-btn" id="back-to-contacts">←</div>
                    <div class="contact-avatar" id="chat-avatar">FC</div>
                    <div class="contact-info">
                        <div class="contact-name" id="chat-contact-name">Nama Kontak</div>
                        <div class="contact-status status-online" id="chat-status">Online</div>
                    </div>
                </div>
                
                <div class="chat-messages" id="chat-messages">
                    <!-- Messages will be dynamically added here -->
                </div>
                
                <div class="chat-input">
                    <input type="text" id="message-input" placeholder="Ketik pesan...">
                    <button class="btn btn-primary" id="send-btn">Kirim</button>
                </div>
            </div>
        </div>
    </div>

    <!-- Notification -->
    <div class="notification" id="notification">
        <div class="notification-title" id="notification-title">Pemberitahuan</div>
        <div class="notification-message" id="notification-message">Isi pemberitahuan</div>
        <div class="notification-actions">
            <button class="btn btn-primary" id="notification-accept">Terima</button>
            <button class="btn btn-secondary" id="notification-deny">Tolak</button>
        </div>
    </div>

    <script>
        // Data storage simulation
        let users = JSON.parse(localStorage.getItem('friendConnectUsers')) || [];
        let currentUser = JSON.parse(localStorage.getItem('friendConnectCurrentUser')) || null;
        let contacts = JSON.parse(localStorage.getItem('friendConnectContacts')) || {};
        let messages = JSON.parse(localStorage.getItem('friendConnectMessages')) || {};
        let connectionRequests = JSON.parse(localStorage.getItem('friendConnectRequests')) || {};
        let bluetoothDevices = JSON.parse(localStorage.getItem('friendConnectBluetoothDevices')) || {};

        // DOM Elements
        const pages = {
            auth: document.getElementById('auth-page'),
            main: document.getElementById('main-page'),
            menu: document.getElementById('menu-page'),
            hostDevice: document.getElementById('host-device-page'),
            scanDevices: document.getElementById('scan-devices-page'),
            chat: document.getElementById('chat-page')
        };

        const menuToggle = document.getElementById('menu-toggle');
        const headerButtons = document.getElementById('header-buttons');
        const loginTab = document.getElementById('login-tab');
        const registerTab = document.getElementById('register-tab');
        const loginForm = document.getElementById('login-form');
        const registerForm = document.getElementById('register-form');
        const loginBtn = document.getElementById('login-btn');
        const registerBtn = document.getElementById('register-btn');
        const contactsList = document.getElementById('contacts-list');
        const hostBluetoothBtn = document.getElementById('host-bluetooth-btn');
        const scanBluetoothBtn = document.getElementById('scan-bluetooth-btn');
        const enableBluetoothBtn = document.getElementById('enable-bluetooth-btn');
        const deviceStatus = document.getElementById('device-status');
        const deviceNameDisplay = document.getElementById('device-name-display');
        const deviceIdDisplay = document.getElementById('device-id-display');
        const startHostingBtn = document.getElementById('start-hosting-btn');
        const stopHostingBtn = document.getElementById('stop-hosting-btn');
        const connectedDevices = document.getElementById('connected-devices');
        const startScanBtn = document.getElementById('start-scan-btn');
        const stopScanBtn = document.getElementById('stop-scan-btn');
        const devicesList = document.getElementById('devices-list');
        const backToContacts = document.getElementById('back-to-contacts');
        const chatMessages = document.getElementById('chat-messages');
        const messageInput = document.getElementById('message-input');
        const sendBtn = document.getElementById('send-btn');
        const chatContactName = document.getElementById('chat-contact-name');
        const chatAvatar = document.getElementById('chat-avatar');
        const notification = document.getElementById('notification');
        const notificationTitle = document.getElementById('notification-title');
        const notificationMessage = document.getElementById('notification-message');
        const notificationAccept = document.getElementById('notification-accept');
        const notificationDeny = document.getElementById('notification-deny');

        // Current state
        let currentChatContact = null;
        let bluetoothEnabled = false;
        let isHosting = false;
        let isScanning = false;
        let currentDeviceId = null;
        let scanInterval = null;

        // Initialize the app
        function initApp() {
            // Check if user is already logged in
            if (currentUser) {
                showPage('main');
                updateHeader();
                loadContacts();
                checkPendingRequests();
            } else {
                showPage('auth');
            }

            setupEventListeners();
        }

        // Set up event listeners
        function setupEventListeners() {
            // Menu toggle
            menuToggle.addEventListener('click', toggleMenu);
            
            // Auth tabs
            loginTab.addEventListener('click', () => switchAuthTab('login'));
            registerTab.addEventListener('click', () => switchAuthTab('register'));
            
            // Auth buttons
            loginBtn.addEventListener('click', handleLogin);
            registerBtn.addEventListener('click', handleRegister);
            
            // Menu buttons
            hostBluetoothBtn.addEventListener('click', () => showPage('hostDevice'));
            scanBluetoothBtn.addEventListener('click', () => showPage('scanDevices'));
            enableBluetoothBtn.addEventListener('click', enableBluetooth);
            
            // Host device buttons
            startHostingBtn.addEventListener('click', startHosting);
            stopHostingBtn.addEventListener('click', stopHosting);
            
            // Scan devices buttons
            startScanBtn.addEventListener('click', startScanning);
            stopScanBtn.addEventListener('click', stopScanning);
            
            // Chat
            backToContacts.addEventListener('click', () => showPage('main'));
            sendBtn.addEventListener('click', sendMessage);
            messageInput.addEventListener('keypress', (e) => {
                if (e.key === 'Enter') sendMessage();
            });
            
            // Notification
            notificationAccept.addEventListener('click', acceptConnection);
            notificationDeny.addEventListener('click', denyConnection);
        }

        // Toggle menu
        function toggleMenu() {
            if (currentUser) {
                showPage('menu');
            } else {
                showNotification('Info', 'Silakan login terlebih dahulu');
            }
        }

        // Switch between login and register forms
        function switchAuthTab(tab) {
            if (tab === 'login') {
                loginTab.classList.add('active');
                registerTab.classList.remove('active');
                loginForm.style.display = 'block';
                registerForm.style.display = 'none';
            } else {
                loginTab.classList.remove('active');
                registerTab.classList.add('active');
                loginForm.style.display = 'none';
                registerForm.style.display = 'block';
            }
        }

        // Handle user login
        function handleLogin() {
            const email = document.getElementById('login-email').value;
            const password = document.getElementById('login-password').value;
            
            if (!email || !password) {
                showNotification('Error', 'Harap isi semua field');
                return;
            }
            
            const user = users.find(u => u.email === email && u.password === password);
            
            if (user) {
                currentUser = user;
                localStorage.setItem('friendConnectCurrentUser', JSON.stringify(currentUser));
                showPage('main');
                updateHeader();
                loadContacts();
                checkPendingRequests();
            } else {
                showNotification('Error', 'Email atau password salah');
            }
        }

        // Handle user registration
        function handleRegister() {
            const email = document.getElementById('register-email').value;
            const password = document.getElementById('register-password').value;
            const confirmPassword = document.getElementById('confirm-password').value;
            
            if (!email || !password || !confirmPassword) {
                showNotification('Error', 'Harap isi semua field');
                return;
            }
            
            if (password !== confirmPassword) {
                showNotification('Error', 'Password tidak cocok');
                return;
            }
            
            if (users.find(u => u.email === email)) {
                showNotification('Error', 'Email sudah terdaftar');
                return;
            }
            
            // Generate user ID
            const userId = generateUserId();
            
            const newUser = {
                id: userId,
                email: email,
                password: password,
                name: email.split('@')[0] // Use part of email as default name
            };
            
            users.push(newUser);
            localStorage.setItem('friendConnectUsers', JSON.stringify(users));
            
            currentUser = newUser;
            localStorage.setItem('friendConnectCurrentUser', JSON.stringify(currentUser));
            
            // Initialize contacts and messages for new user
            if (!contacts[currentUser.id]) contacts[currentUser.id] = [];
            if (!messages[currentUser.id]) messages[currentUser.id] = {};
            localStorage.setItem('friendConnectContacts', JSON.stringify(contacts));
            localStorage.setItem('friendConnectMessages', JSON.stringify(messages));
            
            showPage('main');
            updateHeader();
            loadContacts();
            
            showNotification('Sukses', 'Akun berhasil dibuat!');
        }

        // Generate a simple user ID
        function generateUserId() {
            return 'FC' + Math.random().toString(36).substr(2, 8).toUpperCase();
        }

        // Generate a device ID
        function generateDeviceId() {
            return 'DEV' + Math.random().toString(36).substr(2, 6).toUpperCase();
        }

        // Show specific page
        function showPage(pageName) {
            // Hide all pages
            Object.values(pages).forEach(page => {
                page.classList.remove('active');
            });
            
            // Show requested page
            if (pages[pageName]) {
                pages[pageName].classList.add('active');
                
                // Update page-specific content
                if (pageName === 'hostDevice') {
                    updateHostDevicePage();
                } else if (pageName === 'scanDevices') {
                    updateScanDevicesPage();
                } else if (pageName === 'main') {
                    loadContacts();
                }
            }
            
            updateHeader();
        }

        // Update header based on current page and user state
        function updateHeader() {
            headerButtons.innerHTML = '';
            
            if (currentUser) {
                if (document.getElementById('main-page').classList.contains('active')) {
                    // On main page, show logout button only
                } else if (document.getElementById('menu-page').classList.contains('active')) {
                    // On menu page, show back button
                    headerButtons.innerHTML = '<button class="btn btn-secondary" id="back-btn">Kembali</button>';
                    document.getElementById('back-btn').addEventListener('click', () => showPage('main'));
                } else if (document.getElementById('host-device-page').classList.contains('active') || 
                          document.getElementById('scan-devices-page').classList.contains('active')) {
                    // On device pages, show back to menu button
                    headerButtons.innerHTML = '<button class="btn btn-secondary" id="back-to-menu-btn">Kembali ke Menu</button>';
                    document.getElementById('back-to-menu-btn').addEventListener('click', () => showPage('menu'));
                } else if (document.getElementById('chat-page').classList.contains('active')) {
                    // On chat page, no header buttons needed
                    headerButtons.innerHTML = '';
                }
                
                // Add logout button to all pages except auth
                if (!document.getElementById('auth-page').classList.contains('active')) {
                    const logoutBtn = document.createElement('button');
                    logoutBtn.className = 'btn btn-secondary';
                    logoutBtn.textContent = 'Logout';
                    logoutBtn.style.marginLeft = '10px';
                    logoutBtn.addEventListener('click', handleLogout);
                    headerButtons.appendChild(logoutBtn);
                }
            }
        }

        // Handle user logout
        function handleLogout() {
            currentUser = null;
            localStorage.removeItem('friendConnectCurrentUser');
            showPage('auth');
            
            // Clear form fields
            document.getElementById('login-email').value = '';
            document.getElementById('login-password').value = '';
            document.getElementById('register-email').value = '';
            document.getElementById('register-password').value = '';
            document.getElementById('confirm-password').value = '';
        }

        // Load and display user contacts
        function loadContacts() {
            contactsList.innerHTML = '';
            
            if (!currentUser || !contacts[currentUser.id] || contacts[currentUser.id].length === 0) {
                contactsList.innerHTML = '<p style="text-align: center; padding: 20px; color: var(--text-gray);">Belum ada kontak. Gunakan menu untuk menambahkan teman.</p>';
                return;
            }
            
            contacts[currentUser.id].forEach(contactId => {
                const contactUser = users.find(u => u.id === contactId);
                if (contactUser) {
                    const contactItem = document.createElement('div');
                    contactItem.className = 'contact-item';
                    contactItem.addEventListener('click', () => openChat(contactUser));
                    
                    contactItem.innerHTML = `
                        <div class="contact-avatar">${contactUser.name.charAt(0).toUpperCase()}</div>
                        <div class="contact-info">
                            <div class="contact-name">${contactUser.name}</div>
                            <div class="contact-status status-online">Online</div>
                        </div>
                    `;
                    
                    contactsList.appendChild(contactItem);
                }
            });
        }

        // Enable Bluetooth (simulated)
        function enableBluetooth() {
            if (!bluetoothEnabled) {
                // In a real app, this would request Bluetooth permissions
                showNotification('Bluetooth', 'Mengaktifkan Bluetooth...');
                
                // Simulate Bluetooth enabling
                setTimeout(() => {
                    bluetoothEnabled = true;
                    showNotification('Bluetooth', 'Bluetooth berhasil diaktifkan!');
                }, 1500);
            } else {
                showNotification('Bluetooth', 'Bluetooth sudah aktif');
            }
        }

        // Update host device page
        function updateHostDevicePage() {
            if (currentUser) {
                deviceNameDisplay.textContent = `${currentUser.name}'s Device`;
                
                if (currentDeviceId) {
                    deviceIdDisplay.textContent = currentDeviceId;
                } else {
                    currentDeviceId = generateDeviceId();
                    deviceIdDisplay.textContent = currentDeviceId;
                }
                
                if (isHosting) {
                    deviceStatus.textContent = 'Aktif';
                    deviceStatus.className = 'device-status status-active';
                    startHostingBtn.disabled = true;
                    stopHostingBtn.disabled = false;
                } else {
                    deviceStatus.textContent = 'Tidak Aktif';
                    deviceStatus.className = 'device-status status-inactive';
                    startHostingBtn.disabled = false;
                    stopHostingBtn.disabled = true;
                }
                
                updateConnectedDevices();
            }
        }

        // Start hosting Bluetooth device
        function startHosting() {
            if (!bluetoothEnabled) {
                showNotification('Error', 'Aktifkan Bluetooth terlebih dahulu');
                return;
            }
            
            isHosting = true;
            
            // Create device entry
            if (!bluetoothDevices[currentDeviceId]) {
                bluetoothDevices[currentDeviceId] = {
                    id: currentDeviceId,
                    name: `${currentUser.name}'s Device`,
                    owner: currentUser.id,
                    active: true,
                    connectedDevices: []
                };
            } else {
                bluetoothDevices[currentDeviceId].active = true;
            }
            
            localStorage.setItem('friendConnectBluetoothDevices', JSON.stringify(bluetoothDevices));
            
            updateHostDevicePage();
            showNotification('Sukses', 'Device Bluetooth berhasil dihosting!');
        }

        // Stop hosting Bluetooth device
        function stopHosting() {
            isHosting = false;
            
            if (bluetoothDevices[currentDeviceId]) {
                bluetoothDevices[currentDeviceId].active = false;
                localStorage.setItem('friendConnectBluetoothDevices', JSON.stringify(bluetoothDevices));
            }
            
            updateHostDevicePage();
            showNotification('Info', 'Hosting device dihentikan');
        }

        // Update connected devices list
        function updateConnectedDevices() {
            connectedDevices.innerHTML = '<h3>Perangkat Terhubung:</h3>';
            
            if (!currentDeviceId || !bluetoothDevices[currentDeviceId] || 
                bluetoothDevices[currentDeviceId].connectedDevices.length === 0) {
                connectedDevices.innerHTML += '<p>Tidak ada perangkat terhubung</p>';
                return;
            }
            
            bluetoothDevices[currentDeviceId].connectedDevices.forEach(deviceId => {
                const device = bluetoothDevices[deviceId];
                if (device) {
                    const deviceItem = document.createElement('div');
                    deviceItem.className = 'device-item';
                    deviceItem.innerHTML = `
                        <div>
                            <div>${device.name}</div>
                            <div style="font-size: 12px; color: var(--text-gray);">${device.id}</div>
                        </div>
                        <div class="status-online">Terhubung</div>
                    `;
                    connectedDevices.appendChild(deviceItem);
                }
            });
        }

        // Update scan devices page
        function updateScanDevicesPage() {
            if (isScanning) {
                startScanBtn.disabled = true;
                stopScanBtn.disabled = false;
            } else {
                startScanBtn.disabled = false;
                stopScanBtn.disabled = true;
            }
            
            updateAvailableDevices();
        }

        // Start scanning for Bluetooth devices
        function startScanning() {
            if (!bluetoothEnabled) {
                showNotification('Error', 'Aktifkan Bluetooth terlebih dahulu');
                return;
            }
            
            isScanning = true;
            updateScanDevicesPage();
            
            // Simulate scanning for devices
            scanInterval = setInterval(() => {
                updateAvailableDevices();
            }, 3000);
            
            showNotification('Info', 'Memindai perangkat Bluetooth...');
        }

        // Stop scanning for Bluetooth devices
        function stopScanning() {
            isScanning = false;
            clearInterval(scanInterval);
            updateScanDevicesPage();
            showNotification('Info', 'Pemindaian dihentikan');
        }

        // Update available devices list
        function updateAvailableDevices() {
            devicesList.innerHTML = '';
            
            // Get all active devices except the current user's
            const availableDevices = Object.values(bluetoothDevices).filter(device => 
                device.active && device.owner !== currentUser.id
            );
            
            if (availableDevices.length === 0) {
                devicesList.innerHTML = '<p>Tidak ada perangkat Bluetooth yang tersedia</p>';
                return;
            }
            
            availableDevices.forEach(device => {
                const deviceCard = document.createElement('div');
                deviceCard.className = 'device-card';
                
                deviceCard.innerHTML = `
                    <div class="device-name">${device.name}</div>
                    <div class="device-id">${device.id}</div>
                    <div class="device-actions">
                        <button class="btn btn-primary connect-device-btn" data-device-id="${device.id}">Hubungkan</button>
                    </div>
                `;
                
                devicesList.appendChild(deviceCard);
            });
            
            // Add event listeners to connect buttons
            document.querySelectorAll('.connect-device-btn').forEach(btn => {
                btn.addEventListener('click', (e) => {
                    const deviceId = e.target.getAttribute('data-device-id');
                    connectToDevice(deviceId);
                });
            });
        }

        // Connect to a Bluetooth device
        function connectToDevice(deviceId) {
            const device = bluetoothDevices[deviceId];
            if (!device) {
                showNotification('Error', 'Device tidak ditemukan');
                return;
            }
            
            // Send connection request
            if (!connectionRequests[device.owner]) {
                connectionRequests[device.owner] = [];
            }
            
            connectionRequests[device.owner].push({
                from: currentUser.id,
                deviceId: deviceId,
                timestamp: new Date().toISOString()
            });
            
            localStorage.setItem('friendConnectRequests', JSON.stringify(connectionRequests));
            
            showNotification('Sukses', 'Permintaan koneksi telah dikirim');
        }

        // Check for pending connection requests
        function checkPendingRequests() {
            if (currentUser && connectionRequests[currentUser.id] && connectionRequests[currentUser.id].length > 0) {
                const request = connectionRequests[currentUser.id][0];
                const fromUser = users.find(u => u.id === request.from);
                
                if (fromUser) {
                    showConnectionNotification(fromUser, request.deviceId);
                }
            }
        }

        // Show connection request notification
        function showConnectionNotification(fromUser, deviceId) {
            notificationTitle.textContent = 'Permintaan Koneksi Bluetooth';
            notificationMessage.textContent = `${fromUser.name} ingin terhubung ke device Anda (${deviceId})`;
            notification.classList.add('show');
            
            // Store the current request for later use
            notification.dataset.fromId = fromUser.id;
            notification.dataset.deviceId = deviceId;
        }

        // Accept connection request
        function acceptConnection() {
            const fromId = notification.dataset.fromId;
            const deviceId = notification.dataset.deviceId;
            
            if (fromId && deviceId) {
                // Add to contacts for both users
                if (!contacts[currentUser.id]) contacts[currentUser.id] = [];
                if (!contacts[fromId]) contacts[fromId] = [];
                
                if (!contacts[currentUser.id].includes(fromId)) {
                    contacts[currentUser.id].push(fromId);
                }
                
                if (!contacts[fromId].includes(currentUser.id)) {
                    contacts[fromId].push(currentUser.id);
                }
                
                // Add to connected devices
                if (bluetoothDevices[deviceId] && bluetoothDevices[deviceId].owner === currentUser.id) {
                    if (!bluetoothDevices[deviceId].connectedDevices.includes(fromId)) {
                        bluetoothDevices[deviceId].connectedDevices.push(fromId);
                    }
                }
                
                // Initialize message storage for this connection
                if (!messages[currentUser.id]) messages[currentUser.id] = {};
                if (!messages[fromId]) messages[fromId] = {};
                
                if (!messages[currentUser.id][fromId]) messages[currentUser.id][fromId] = [];
                if (!messages[fromId][currentUser.id]) messages[fromId][currentUser.id] = [];
                
                // Remove the request
                if (connectionRequests[currentUser.id]) {
                    connectionRequests[currentUser.id] = connectionRequests[currentUser.id].filter(
                        req => req.from !== fromId
                    );
                }
                
                // Save to localStorage
                localStorage.setItem('friendConnectContacts', JSON.stringify(contacts));
                localStorage.setItem('friendConnectMessages', JSON.stringify(messages));
                localStorage.setItem('friendConnectRequests', JSON.stringify(connectionRequests));
                localStorage.setItem('friendConnectBluetoothDevices', JSON.stringify(bluetoothDevices));
                
                // Hide notification
                notification.classList.remove('show');
                
                // Show success message
                showNotification('Sukses', 'Koneksi Bluetooth berhasil dibuat!');
                
                // Reload contacts
                loadContacts();
                
                // Update connected devices if on host device page
                if (document.getElementById('host-device-page').classList.contains('active')) {
                    updateConnectedDevices();
                }
            }
        }

        // Deny connection request
        function denyConnection() {
            const fromId = notification.dataset.fromId;
            
            if (fromId && connectionRequests[currentUser.id]) {
                connectionRequests[currentUser.id] = connectionRequests[currentUser.id].filter(
                    req => req.from !== fromId
                );
                
                localStorage.setItem('friendConnectRequests', JSON.stringify(connectionRequests));
                
                notification.classList.remove('show');
                showNotification('Info', 'Permintaan koneksi ditolak');
            }
        }

        // Open chat with a contact
        function openChat(contact) {
            currentChatContact = contact;
            chatContactName.textContent = contact.name;
            chatAvatar.textContent = contact.name.charAt(0).toUpperCase();
            
            showPage('chat');
            loadChatMessages();
        }

        // Load chat messages
        function loadChatMessages() {
            chatMessages.innerHTML = '';
            
            if (!currentUser || !currentChatContact) return;
            
            const userMessages = messages[currentUser.id] && messages[currentUser.id][currentChatContact.id]
                ? messages[currentUser.id][currentChatContact.id] 
                : [];
            
            if (userMessages.length === 0) {
                chatMessages.innerHTML = '<p style="text-align: center; padding: 20px; color: var(--text-gray);">Belum ada pesan. Mulai percakapan!</p>';
                return;
            }
            
            userMessages.forEach(msg => {
                const messageDiv = document.createElement('div');
                messageDiv.className = `message ${msg.sender === currentUser.id ? 'sent' : 'received'}`;
                
                const time = new Date(msg.timestamp).toLocaleTimeString([], { hour: '2-digit', minute: '2-digit' });
                
                messageDiv.innerHTML = `
                    <div class="message-text">${msg.text}</div>
                    <div class="message-time">${time}</div>
                `;
                
                chatMessages.appendChild(messageDiv);
            });
            
            // Scroll to bottom
            chatMessages.scrollTop = chatMessages.scrollHeight;
        }

        // Send a message
        function sendMessage() {
            const messageText = messageInput.value.trim();
            
            if (!messageText || !currentUser || !currentChatContact) return;
            
            // Create message object
            const message = {
                text: messageText,
                sender: currentUser.id,
                timestamp: new Date().toISOString()
            };
            
            // Save message for current user
            if (!messages[currentUser.id]) messages[currentUser.id] = {};
            if (!messages[currentUser.id][currentChatContact.id]) messages[currentUser.id][currentChatContact.id] = [];
            messages[currentUser.id][currentChatContact.id].push(message);
            
            // Save message for contact (simulating message delivery via Bluetooth)
            if (!messages[currentChatContact.id]) messages[currentChatContact.id] = {};
            if (!messages[currentChatContact.id][currentUser.id]) messages[currentChatContact.id][currentUser.id] = [];
            messages[currentChatContact.id][currentUser.id].push(message);
            
            // Save to localStorage
            localStorage.setItem('friendConnectMessages', JSON.stringify(messages));
            
            // Clear input and reload messages
            messageInput.value = '';
            loadChatMessages();
            
            // In a real app, you would send the message via Bluetooth here
        }

        // Show notification
        function showNotification(title, message) {
            notificationTitle.textContent = title;
            notificationMessage.textContent = message;
            
            // Hide action buttons for non-connection notifications
            if (title !== 'Permintaan Koneksi Bluetooth') {
                notificationAccept.style.display = 'none';
                notificationDeny.style.display = 'none';
            } else {
                notificationAccept.style.display = 'inline-block';
                notificationDeny.style.display = 'inline-block';
            }
            
            notification.classList.add('show');
            
            // Auto-hide after 3 seconds for non-connection notifications
            if (title !== 'Permintaan Koneksi Bluetooth') {
                setTimeout(() => {
                    notification.classList.remove('show');
                }, 3000);
            }
        }

        // Initialize the app when DOM is loaded
        document.addEventListener('DOMContentLoaded', initApp);
    </script>
</body>
</html>
