<!DOCTYPE html>
<html lang="hi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Cricket Schedule & Points Table Portal</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        .hidden-section { display: none; }
    </style>
</head>
<body class="bg-slate-100 font-sans text-gray-800">

    <!-- Navbar -->
    <nav class="bg-emerald-700 text-white p-4 shadow-md flex justify-between items-center">
        <h1 class="text-xl font-bold">🏏 Cricket Portal</h1>
        <div id="nav-user-info" class="flex items-center gap-2 hidden-section flex-wrap">
            <span id="nav-phone" class="text-xs bg-emerald-800 px-2 py-1 rounded"></span>
            <span id="nav-wallet" class="text-xs bg-amber-500 text-slate-900 font-bold px-2 py-1 rounded"></span>
            <span id="nav-rupees" class="text-xs bg-blue-600 text-white font-bold px-2 py-1 rounded"></span>
            <span id="nav-dollars" class="text-xs bg-teal-600 text-white font-bold px-2 py-1 rounded"></span>
            <span id="nav-diamonds" class="text-xs bg-purple-600 text-white font-bold px-2 py-1 rounded"></span>
            <button onclick="logout()" class="bg-red-600 px-2 py-1 text-xs rounded hover:bg-red-700">Logout</button>
        </div>
    </nav>

    <div class="container mx-auto p-4 max-w-5xl">

        <!-- 1. LOGIN SECTION -->
        <div id="login-section" class="bg-white p-6 rounded-lg shadow-md max-w-md mx-auto mt-10">
            <h2 class="text-2xl font-bold text-center mb-4 text-emerald-700">Login / Register</h2>
            <div class="mb-4">
                <label class="block text-sm font-medium mb-1">Mobile Number:</label>
                <input type="text" id="login-phone" placeholder="Enter Mobile Number" class="w-full p-2 border rounded focus:ring-2 focus:ring-emerald-500">
            </div>
            <div class="mb-4">
                <label class="block text-sm font-medium mb-1">Unique User ID (Exactly 5 letters/chars):</label>
                <input type="text" id="login-userid" maxlength="5" placeholder="e.g. df43h" class="w-full p-2 border rounded focus:ring-2 focus:ring-emerald-500">
            </div>
            <button onclick="handleLogin()" class="w-full bg-emerald-600 text-white py-2 rounded font-bold hover:bg-emerald-700">Login</button>
            <p class="text-xs text-gray-500 mt-3 text-center">Note: New users get 180 Points Bonus & 150 Dollars Bonus!</p>
        </div>

        <!-- 2. MAIN DASHBOARD -->
        <div id="dashboard-section" class="hidden-section">
            <!-- Tabs -->
            <div class="flex flex-wrap gap-2 mb-6 border-b pb-2">
                <button onclick="switchTab('matches')" class="px-3 py-2 bg-emerald-600 text-white rounded font-medium text-sm tab-btn" id="btn-matches">Match Schedule</button>
                <button onclick="switchTab('points')" class="px-3 py-2 bg-gray-200 text-gray-700 rounded font-medium text-sm tab-btn" id="btn-points">Points Table</button>
                <button onclick="switchTab('spin')" class="px-3 py-2 bg-gray-200 text-gray-700 rounded font-medium text-sm tab-btn" id="btn-spin">🎡 Lucky Spin</button>
                <button onclick="switchTab('diamonds')" class="px-3 py-2 bg-gray-200 text-gray-700 rounded font-medium text-sm tab-btn" id="btn-diamonds">💎 Buy Diamonds (Real ₹)</button>
                <button onclick="switchTab('shop')" class="px-3 py-2 bg-gray-200 text-gray-700 rounded font-medium text-sm tab-btn" id="btn-shop">🛍️ Shop & Vouchers</button>
                <button onclick="switchTab('subscription')" class="px-3 py-2 bg-gray-200 text-gray-700 rounded font-medium text-sm tab-btn" id="btn-subscription">Subscriptions</button>
                <button onclick="switchTab('addcoins')" class="px-3 py-2 bg-gray-200 text-gray-700 rounded font-medium text-sm tab-btn" id="btn-addcoins">Redeem Voucher</button>
                <button onclick="switchTab('admin')" class="px-3 py-2 bg-gray-200 text-gray-700 rounded font-medium text-sm tab-btn" id="btn-admin">Admin Panel</button>
            </div>

            <!-- TAB 1: MATCH SCHEDULE -->
            <div id="tab-matches" class="space-y-4">
                <h3 class="text-xl font-bold text-emerald-800">Scheduled Matches</h3>
                <div id="matches-list" class="space-y-4"></div>
            </div>

            <!-- TAB 2: POINTS TABLE -->
            <div id="tab-points" class="hidden-section space-y-6">
                <h3 class="text-xl font-bold text-emerald-800">ICC Points Table</h3>
                <div id="points-tables-container" class="space-y-6"></div>
            </div>

            <!-- TAB 3: LUCKY SPIN WHEEL -->
            <div id="tab-spin" class="hidden-section bg-white p-6 rounded-lg shadow-md max-w-lg mx-auto text-center space-y-4">
                <h3 class="text-2xl font-bold text-pink-700">🎡 Lucky Spin Wheel</h3>
                <p class="text-sm text-gray-600">Rozana sirf 3 spins milte hain! Jeetein Dollars, ₹ ya fir सावधान (-50 Points Minus bhi ho sakte hain).</p>
                <div class="p-6 bg-pink-50 border border-pink-200 rounded-lg">
                    <p id="spin-result-display" class="text-xl font-extrabold text-purple-900 mb-4">Spin karne ke liye button dabayein!</p>
                    <p id="spin-limit-info" class="text-xs font-semibold text-red-600 mb-4"></p>
                    <button onclick="doSpinWheel()" id="spin-btn" class="bg-pink-600 text-white px-6 py-3 rounded-full font-bold shadow hover:bg-pink-700 text-base">SPIN NOW</button>
                </div>
            </div>

            <!-- TAB 4: BUY DIAMONDS (REAL PAYMENT -> Ashadkazitola@gmail.com) -->
            <div id="tab-diamonds" class="hidden-section bg-white p-6 rounded-lg shadow-md max-w-xl mx-auto space-y-4">
                <h3 class="text-xl font-bold text-purple-800">💎 Buy Diamonds & Balance via Google Play</h3>
                <p class="text-sm text-gray-600">Real money (Google Play Billing / UPI) ke zariye Diamonds kharidein. Yeh sabhi transactions seedhe <b>Ashadkazitola@gmail.com</b> ke merchant account par process honge.</p>
                
                <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
                    <div class="border p-4 rounded-lg bg-purple-50 flex flex-col justify-between">
                        <div>
                            <h4 class="font-bold text-purple-900">Starter Diamond Pack</h4>
                            <p class="text-xs text-gray-600 mb-2">50 Diamonds + ₹50 Wallet Credit</p>
                            <p class="text-sm font-bold text-emerald-700 mb-3">Price: ₹50</p>
                        </div>
                        <button onclick="buyDiamondsReal(50, 50, 'Starter Pack')" class="bg-purple-600 text-white py-2 rounded text-sm font-bold hover:bg-purple-700">Pay ₹50 via Play Store</button>
                    </div>

                    <div class="border p-4 rounded-lg bg-purple-50 flex flex-col justify-between">
                        <div>
                            <h4 class="font-bold text-purple-900">Pro Diamond Pack</h4>
                            <p class="text-xs text-gray-600 mb-2">150 Diamonds + ₹150 Wallet Credit</p>
                            <p class="text-sm font-bold text-emerald-700 mb-3">Price: ₹149</p>
                        </div>
                        <button onclick="buyDiamondsReal(150, 150, 'Pro Pack')" class="bg-purple-600 text-white py-2 rounded text-sm font-bold hover:bg-purple-700">Pay ₹149 via Play Store</button>
                    </div>
                </div>
            </div>

            <!-- TAB 5: SHOP & VOUCHERS -->
            <div id="tab-shop" class="hidden-section bg-white p-6 rounded-lg shadow-md space-y-4">
                <h3 class="text-xl font-bold text-emerald-800">🛍️ Shop & Vouchers</h3>
                <div id="shop-voucher-box" style="display: none;" class="bg-amber-50 border border-amber-300 p-4 rounded mb-4">
                    <p class="text-xs text-gray-600 mb-1">Aapka Generated Code:</p>
                    <div class="flex items-center justify-between bg-white p-2 border rounded">
                        <span id="shop-gen-code" class="font-bold text-emerald-700 text-base select-all"></span>
                        <button onclick="copyShopCode()" class="bg-emerald-600 text-white text-xs px-3 py-1 rounded hover:bg-emerald-700">Copy Code</button>
                    </div>
                </div>
                <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
                    <div class="p-4 border rounded shadow-sm bg-gray-50">
                        <p class="font-bold text-lg text-emerald-800">400 Points Voucher</p>
                        <p class="text-sm text-gray-600 mb-3">Cost: ₹50 Balance</p>
                        <button onclick="buyVoucherOrSubFromBalance(50, 'points', 400, '400 Points Voucher')" class="bg-emerald-600 text-white px-4 py-2 rounded text-sm font-bold hover:bg-emerald-700 w-full">Redeem for ₹50</button>
                    </div>
                </div>
            </div>

            <!-- TAB 6: SUBSCRIPTION -->
            <div id="tab-subscription" class="hidden-section bg-white p-6 rounded-lg shadow-md space-y-4">
                <h3 class="text-xl font-bold mb-4 text-emerald-800">Active Admin Subscription</h3>
                <div id="subscription-status" class="mb-6 p-4 bg-amber-50 border border-amber-200 rounded"></div>
                <h4 class="text-lg font-semibold mb-3">Buy / Activate Subscription & Get Dollars Reward</h4>
                
                <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
                    <div class="p-4 border rounded shadow-sm bg-blue-50">
                        <p class="font-bold">30 Minutes Subscription</p>
                        <p class="text-xs text-gray-600 mb-2">Reward: +15 Dollars Free</p>
                        <p class="text-sm font-bold text-emerald-700 mb-2">Cost: 149 Points</p>
                        <button onclick="buySubscription('30min', 149, 15)" class="w-full bg-blue-600 text-white py-1 rounded text-sm font-bold">Buy Now</button>
                    </div>
                    <div class="p-4 border rounded shadow-sm bg-blue-50">
                        <p class="font-bold">Half Monthly Subscription</p>
                        <p class="text-xs text-gray-600 mb-2">Reward: +30 Dollars Free</p>
                        <p class="text-sm font-bold text-emerald-700 mb-2">Cost: 1500 Points</p>
                        <button onclick="buySubscription('half_monthly', 1500, 30)" class="w-full bg-blue-600 text-white py-1 rounded text-sm font-bold">Buy Now</button>
                    </div>
                    <div class="p-4 border rounded shadow-sm bg-blue-50">
                        <p class="font-bold">Monthly Subscription</p>
                        <p class="text-xs text-gray-600 mb-2">Reward: +50 Dollars Free</p>
                        <p class="text-sm font-bold text-emerald-700 mb-2">Cost: 3000 Points</p>
                        <button onclick="buySubscription('monthly', 3000, 50)" class="w-full bg-blue-600 text-white py-1 rounded text-sm font-bold">Buy Now</button>
                    </div>
                    <div class="p-4 border rounded shadow-sm bg-blue-50">
                        <p class="font-bold">Yearly Subscription</p>
                        <p class="text-xs text-gray-600 mb-2">Reward: +90 Dollars Free</p>
                        <p class="text-sm font-bold text-emerald-700 mb-2">Cost: 8000 Points</p>
                        <button onclick="buySubscription('yearly', 8000, 90)" class="w-full bg-blue-600 text-white py-1 rounded text-sm font-bold">Buy Now</button>
                    </div>
                </div>
            </div>

            <!-- TAB 7: REDEEM VOUCHER -->
            <div id="tab-addcoins" class="hidden-section bg-white p-6 rounded-lg shadow-md max-w-lg mx-auto">
                <h3 class="text-xl font-bold mb-2 text-emerald-800">🎟️ Redeem Personal Voucher Code</h3>
                <div class="mb-4">
                    <label class="block text-sm font-medium mb-1">Enter Your Voucher Code:</label>
                    <input type="text" id="user-voucher-code" placeholder="e.g. VCH-XYZ-1234" class="w-full p-2 border rounded uppercase">
                </div>
                <button onclick="userRedeemVoucher()" class="w-full bg-amber-600 text-white py-2 rounded font-bold">Redeem Voucher</button>
            </div>

            <!-- TAB 8: ADMIN PANEL -->
            <div id="tab-admin" class="hidden-section space-y-6">
                <div class="bg-white p-6 rounded-lg shadow-md border-t-4 border-emerald-600">
                    <h3 class="text-xl font-bold text-emerald-800 mb-4">👑 Admin Panel</h3>
                    <div class="border-b pb-6 mb-6 bg-emerald-50 p-4 rounded border border-emerald-200">
                        <h4 class="font-semibold mb-3 text-lg text-emerald-900">🔒 Generate Voucher Code</h4>
                        <div class="grid grid-cols-1 md:grid-cols-2 gap-4 mb-3">
                            <div>
                                <label class="block text-sm font-medium">Target User ID/Mobile:</label>
                                <input type="text" id="admin-vch-target" placeholder="User ID" class="w-full p-2 border rounded bg-white">
                            </div>
                            <div>
                                <label class="block text-sm font-medium">Points Amount:</label>
                                <input type="number" id="admin-vch-points" placeholder="e.g. 1500" class="w-full p-2 border rounded bg-white">
                            </div>
                        </div>
                        <button onclick="adminGenerateVoucher()" class="bg-emerald-700 text-white px-4 py-2 rounded font-bold text-sm">Generate Code</button>
                        <div id="generated-voucher-box" style="display: none;" class="mt-3 bg-white p-3 border rounded text-sm">
                            <span id="display-gen-code" class="font-bold text-emerald-700 text-base select-all"></span>
                        </div>
                    </div>
                </div>
            </div>

        </div>
    </div>

    <!-- JavaScript Logic -->
    <script>
        const DB_KEY = "cricket_portal_db_v13";
        let currentUserPhone = localStorage.getItem("current_logged_user") || null;
        let currentDeviceId = localStorage.getItem("device_id");

        if (!currentDeviceId) {
            currentDeviceId = "dev_" + Math.random().toString(36).substring(2, 9);
            localStorage.setItem("device_id", currentDeviceId);
        }

        function getDB() {
            let data = localStorage.getItem(DB_KEY);
            if (!data) {
                let initial = {
                    users: {},       
                    userIDs: {},     
                    devices: {},
                    matches: [],
                    pointsTable: {}, 
                    subscription: { activeUntil: 0 },
                    customVouchers: {},
                    spinData: {}
                };
                localStorage.setItem(DB_KEY, JSON.stringify(initial));
                return initial;
            }
            return JSON.parse(data);
        }

        function saveDB(db) {
            localStorage.setItem(DB_KEY, JSON.stringify(db));
        }

        window.onload = function() {
            if (currentUserPhone) {
                let db = getDB();
                if (db.users[currentUserPhone]) {
                    initDashboard();
                } else {
                    currentUserPhone = null;
                    localStorage.removeItem("current_logged_user");
                }
            }
        };

        function handleLogin() {
            let phone = document.getElementById("login-phone").value.trim();
            let userid = document.getElementById("login-userid").value.trim().toLowerCase();

            if (!phone || !userid) {
                alert("Please enter both Mobile Number and User ID.");
                return;
            }

            if (userid.length !== 5) {
                alert("User ID must be exactly 5 letters/characters long.");
                return;
            }

            let db = getDB();
            if (!db.devices[currentDeviceId]) {
                db.devices[currentDeviceId] = [];
            }

            let devicePhones = db.devices[currentDeviceId];
            if (!devicePhones.includes(phone) && devicePhones.length >= 3) {
                alert("Maximum 3 numbers allowed per device!");
                return;
            }

            let walletBalance = 180; // 180 points bonus for new/existing registration
            let rupeesBalance = 0;
            let dollarsBalance = 150; // 150 dollars bonus
            let diamondsCount = 0;

            if (db.userIDs[userid] && db.userIDs[userid].wallet !== undefined) {
                walletBalance = db.userIDs[userid].wallet;
                rupeesBalance = db.userIDs[userid].rupees || 0;
                dollarsBalance = db.userIDs[userid].dollars !== undefined ? db.userIDs[userid].dollars : 150;
                diamondsCount = db.userIDs[userid].diamonds || 0;
            } 
            else if (db.users[phone] && db.users[phone].wallet !== undefined) {
                walletBalance = db.users[phone].wallet;
                rupeesBalance = db.users[phone].rupees || 0;
                dollarsBalance = db.users[phone].dollars !== undefined ? db.users[phone].dollars : 150;
                diamondsCount = db.users[phone].diamonds || 0;
            }

            db.users[phone] = { phone: phone, userid: userid, wallet: walletBalance, rupees: rupeesBalance, dollars: dollarsBalance, diamonds: diamondsCount };
            
            if (!db.userIDs[userid]) {
                db.userIDs[userid] = db.users[phone];
            } else {
                db.userIDs[userid].phone = phone;
                db.users[phone] = db.userIDs[userid];
            }

            if (!devicePhones.includes(phone)) {
                devicePhones.push(phone);
            }

            saveDB(db);
            currentUserPhone = phone;
            localStorage.setItem("current_logged_user", currentUserPhone);
            initDashboard();
        }

        function logout() {
            currentUserPhone = null;
            localStorage.removeItem("current_logged_user");
            document.getElementById("login-section").classList.remove("hidden-section");
            document.getElementById("dashboard-section").classList.add("hidden-section");
            document.getElementById("nav-user-info").classList.add("hidden-section");
        }

        function initDashboard() {
            document.getElementById("login-section").classList.add("hidden-section");
            document.getElementById("dashboard-section").classList.remove("hidden-section");
            document.getElementById("nav-user-info").classList.remove("hidden-section");

            let db = getDB();
            let user = db.users[currentUserPhone];

            if (user && db.userIDs[user.userid]) {
                user.wallet = db.userIDs[user.userid].wallet;
                user.rupees = db.userIDs[user.userid].rupees || 0;
                user.dollars = db.userIDs[user.userid].dollars || 0;
                user.diamonds = db.userIDs[user.userid].diamonds || 0;
            }

            document.getElementById("nav-phone").innerText = `📞 ${user.phone}`;
            document.getElementById("nav-wallet").innerText = `💰 Pts: ${user.wallet}`;
            document.getElementById("nav-rupees").innerText = `₹ ${user.rupees}`;
            document.getElementById("nav-dollars").innerText = `\$ ${user.dollars}`;
            document.getElementById("nav-diamonds").innerText = `💎 ${user.diamonds}`;

            renderSubscriptionStatus();
            updateSpinLimitUI();
        }

        function switchTab(tabName) {
            let db = getDB();
            let now = new Date().getTime();
            let isAdminActive = db.subscription.activeUntil > now;

            if (tabName === 'admin' && !isAdminActive) {
                alert("Active subscription required to open Admin Panel!");
                return;
            }

            ['matches', 'points', 'spin', 'diamonds', 'shop', 'subscription', 'addcoins', 'admin'].forEach(t => {
                let tabElem = document.getElementById(`tab-${t}`);
                let btnElem = document.getElementById(`btn-${t}`);
                if(tabElem) tabElem.classList.add("hidden-section");
                if(btnElem) {
                    btnElem.classList.remove("bg-emerald-600", "text-white");
                    btnElem.classList.add("bg-gray-200", "text-gray-700");
                }
            });

            document.getElementById(`tab-${tabName}`).classList.remove("hidden-section");
            document.getElementById(`btn-${tabName}`).classList.remove("bg-gray-200", "text-gray-700");
            document.getElementById(`btn-${tabName}`).classList.add("bg-emerald-600", "text-white");
        }

        // --- LUCKY SPIN SYSTEM ---
        function getTodayDateString() {
            let d = new Date();
            return d.getFullYear() + '-' + (d.getMonth()+1) + '-' + d.getDate();
        }

        function updateSpinLimitUI() {
            let db = getDB();
            let today = getTodayDateString();
            if (!db.spinData[currentUserPhone]) {
                db.spinData[currentUserPhone] = { date: today, count: 0 };
            }
            if (db.spinData[currentUserPhone].date !== today) {
                db.spinData[currentUserPhone] = { date: today, count: 0 };
            }
            saveDB(db);

            let spinsLeft = 3 - db.spinData[currentUserPhone].count;
            let limitInfo = document.getElementById("spin-limit-info");
            let spinBtn = document.getElementById("spin-btn");

            limitInfo.innerText = `Aaj ke bache hue spins: ${spinsLeft} / 3`;
            if (spinsLeft <= 0) {
                spinBtn.disabled = true;
                spinBtn.classList.add("opacity-50", "cursor-not-allowed");
            } else {
                spinBtn.disabled = false;
                spinBtn.classList.remove("opacity-50", "cursor-not-allowed");
            }
        }

        function doSpinWheel() {
            let db = getDB();
            let today = getTodayDateString();
            if (!db.spinData[currentUserPhone]) {
                db.spinData[currentUserPhone] = { date: today, count: 0 };
            }
            if (db.spinData[currentUserPhone].date !== today) {
                db.spinData[currentUserPhone] = { date: today, count: 0 };
            }

            if (db.spinData[currentUserPhone].count >= 3) {
                alert("Aap aaj ke 3 spin poore kar chuke hain!");
                updateSpinLimitUI();
                return;
            }

            db.spinData[currentUserPhone].count++;
            saveDB(db);

            // Spin outcomes: 25$, 25₹, 0₹, 30₹, 100₹, 800₹, 0₹, -50 Points
            const outcomes = [
                { type: 'dollar', val: 25, text: '🎉 Badhai ho! Aapne jeete: 25 Dollars ($)' },
                { type: 'rupee', val: 25, text: '🎉 Badhai ho! Aapne jeete: ₹25' },
                { type: 'rupee', val: 0, text: 'Oops! Kismat kharab, mila: ₹0' },
                { type: 'rupee', val: 30, text: '🎉 Badhai ho! Aapne jeete: ₹30' },
                { type: 'rupee', val: 100, text: '🔥 Shandar! Aapne jeete: ₹100' },
                { type: 'rupee', val: 800, text: '🏆 Jackpot! Aapne jeete: ₹800' },
                { type: 'rupee', val: 0, text: 'Better luck next time! Mila: ₹0' },
                { type: 'minus_points', val: 50, text: '⚠️ Oh no! Teer -50 Points par ruka, wallet se 50 Points cut gaye!' }
            ];

            let randomOutcome = outcomes[Math.floor(Math.random() * outcomes.length)];
            let user = db.users[currentUserPhone];

            if (randomOutcome.type === 'dollar') {
                user.dollars = (user.dollars || 0) + randomOutcome.val;
            } else if (randomOutcome.type === 'rupee') {
                user.rupees = (user.rupees || 0) + randomOutcome.val;
            } else if (randomOutcome.type === 'minus_points') {
                user.wallet = Math.max(0, user.wallet - randomOutcome.val);
            }

            if (db.userIDs[user.userid]) {
                db.userIDs[user.userid].wallet = user.wallet;
                db.userIDs[user.userid].rupees = user.rupees;
                db.userIDs[user.userid].dollars = user.dollars;
            }

            saveDB(db);
            initDashboard();
            document.getElementById("spin-result-display").innerText = randomOutcome.text;
            updateSpinLimitUI();
        }

        // --- REAL PAYMENT GATEWAY (GOOGLE PLAY) ---
        function buyDiamondsReal(amountInRupees, diamondsReward, packageName) {
            let confirmPayment = confirm(`[Google Play Billing - Ashadkazitola@gmail.com]\nAap "${packageName}" khareed rahe hain jiska muly ₹${amountInRupees} hai. Kya aap payment complete karna chahte hain?`);
            if (!confirmPayment) return;

            let db = getDB();
            let user = db.users[currentUserPhone];

            user.rupees = (user.rupees || 0) + amountInRupees;
            user.diamonds = (user.diamonds || 0) + diamondsReward;

            if (db.userIDs[user.userid]) {
                db.userIDs[user.userid].rupees = user.rupees;
                db.userIDs[user.userid].diamonds = user.diamonds;
            }

            saveDB(db);
            initDashboard();
            alert(`Payment Successful! ${diamondsReward} Diamonds aur ₹${amountInRupees} add ho gaye hain.`);
        }

        function buyVoucherOrSubFromBalance(costInRupees, itemType, pointsValue, itemName) {
            let db = getDB();
            let user = db.users[currentUserPhone];

            if (!user.rupees || user.rupees < costInRupees) {
                alert(`Paryapt balance nahi hai! (Required: ₹${costInRupees})`);
                return;
            }

            user.rupees -= costInRupees;
            if (db.userIDs[user.userid]) db.userIDs[user.userid].rupees = user.rupees;

            let voucherCode = "VCH-" + user.userid.toUpperCase() + "-" + Math.floor(1000 + Math.random() * 9000);
            if (!db.customVouchers) db.customVouchers = {};
            db.customVouchers[voucherCode] = { target: user.userid.toLowerCase(), points: pointsValue, used: false };

            saveDB(db);
            initDashboard();

            document.getElementById("shop-voucher-box").style.display = "block";
            document.getElementById("shop-gen-code").innerText = voucherCode;
            alert(`Safaltapurvak "${itemName}" kharid liya gaya hai!`);
        }

        function copyShopCode() {
            let codeText = document.getElementById("shop-gen-code").innerText;
            navigator.clipboard.writeText(codeText).then(() => alert("Code copy ho gaya hai!"));
        }

        function adminGenerateVoucher() {
            let db = getDB();
            let targetUser = document.getElementById("admin-vch-target").value.trim().toLowerCase();
            let points = parseInt(document.getElementById("admin-vch-points").value);

            if (!targetUser || isNaN(points) || points <= 0) {
                alert("Valid target user aur points bharein!");
                return;
            }

            if (!db.customVouchers) db.customVouchers = {};
            let randomCode = "VCH-" + targetUser.toUpperCase() + "-" + Math.floor(1000 + Math.random() * 9000);
            db.customVouchers[randomCode] = { target: targetUser, points: points, used: false };
            saveDB(db);

            document.getElementById("generated-voucher-box").style.display = "block";
            document.getElementById("display-gen-code").innerText = randomCode;
            alert("Voucher code generated successfully!");
        }

        function userRedeemVoucher() {
            let db = getDB();
            let codeInput = document.getElementById("user-voucher-code").value.trim().toUpperCase();

            if (!codeInput || !db.customVouchers || !db.customVouchers[codeInput]) {
                alert("Yeh voucher code galat ya astitva mein nahi hai!");
                return;
            }

            let vchData = db.customVouchers[codeInput];
            if (vchData.used) {
                alert("Yeh voucher code pehle hi use kiya ja chuka hai!");
                return;
            }

            let currentUser = db.users[currentUserPhone];
            if (currentUser.phone.toLowerCase() !== vchData.target && currentUser.userid.toLowerCase() !== vchData.target) {
                alert("Yeh voucher code sirf nirdharit user ke liye valid hai!");
                return;
            }

            currentUser.wallet += vchData.points;
            if (db.userIDs[currentUser.userid]) db.userIDs[currentUser.userid].wallet = currentUser.wallet;
            vchData.used = true;

            saveDB(db);
            alert(`Badhai ho! Aapke wallet mein ${vchData.points} Points jud gaye hain.`);
            document.getElementById("user-voucher-code").value = "";
            initDashboard();
        }

        // SUBSCRIPTIONS WITH DOLLAR REWARDS
        function buySubscription(plan, cost, dollarReward) {
            let db = getDB();
            let user = db.users[currentUserPhone];
            let currentWallet = user ? user.wallet : 0;

            if (currentWallet < cost) {
                alert("Insufficient points in wallet!");
                return;
            }

            currentWallet -= cost;
            user.wallet = currentWallet;
            user.dollars = (user.dollars || 0) + dollarReward;

            if (db.userIDs[user.userid]) {
                db.userIDs[user.userid].wallet = currentWallet;
                db.userIDs[user.userid].dollars = user.dollars;
            }

            let durationMs = 0;
            if (plan === '30min') durationMs = 30 * 60 * 1000;
            else if (plan === 'half_monthly') durationMs = 15 * 24 * 60 * 60 * 1000;
            else if (plan === 'monthly') durationMs = 30 * 24 * 60 * 60 * 1000;
            else if (plan === 'yearly') durationMs = 365 * 24 * 60 * 60 * 1000;

            let now = new Date().getTime();
            if (db.subscription.activeUntil > now) db.subscription.activeUntil += durationMs;
            else db.subscription.activeUntil = now + durationMs;

            saveDB(db);
            alert(`Subscription activated successfully! Aapko ${dollarReward} Dollars bonus mile hain.`);
            initDashboard();
        }

        function renderSubscriptionStatus() {
            let db = getDB();
            let now = new Date().getTime();
            let statusBox = document.getElementById("subscription-status");
            if (db.subscription.activeUntil > now) {
                let timeLeft = Math.ceil((db.subscription.activeUntil - now) / (1000 * 60));
                statusBox.innerHTML = `<p class="text-emerald-700 font-bold">Status: Active</p><p class="text-sm">Expires in approximately ${timeLeft} minutes.</p>`;
            } else {
                statusBox.innerHTML = `<p class="text-red-600 font-bold">Status: Inactive</p>`;
            }
        }
    </script>
</body>
</html>
