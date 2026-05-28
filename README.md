<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Mobile ATM Simulator</title>
    <style>
        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            font-family: 'Segoe UI', Roboto, Helvetica, Arial, sans-serif;
        }
        body {
            background-color: #121214;
            color: #ffffff;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            padding: 15px;
        }
        .atm-container {
            width: 100%;
            max-width: 400px;
            background: #1e1e24;
            border-radius: 20px;
            padding: 25px;
            box-shadow: 0 10px 25px rgba(0,0,0,0.5);
            border: 1px solid #2e2e38;
        }
        h2 {
            text-align: center;
            color: #4caf50;
            margin-bottom: 20px;
            font-size: 24px;
            letter-spacing: 1px;
        }
        .screen-display {
            background-color: #0f0f12;
            border: 2px solid #333;
            border-radius: 12px;
            padding: 20px;
            min-height: 120px;
            margin-bottom: 25px;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            text-align: center;
        }
        .screen-text {
            font-size: 18px;
            color: #00ffcc;
            font-family: 'Courier New', Courier, monospace;
            font-weight: bold;
            line-height: 1.4;
        }
        .input-group {
            margin-bottom: 20px;
            display: none; /* Hidden by default, shown when needed */
        }
        label {
            display: block;
            margin-bottom: 8px;
            color: #aaa;
            font-size: 14px;
        }
        input {
            width: 100%;
            padding: 14px;
            border-radius: 10px;
            border: 1px solid #444;
            background: #2a2a35;
            color: #fff;
            font-size: 18px;
            text-align: center;
            outline: none;
            transition: border 0.3s;
        }
        input:focus {
            border-color: #4caf50;
        }
        .grid-menu {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 15px;
        }
        button {
            padding: 15px;
            border: none;
            border-radius: 12px;
            font-size: 15px;
            font-weight: bold;
            cursor: pointer;
            transition: transform 0.1s, filter 0.2s;
            color: white;
        }
        button:active {
            transform: scale(0.96);
        }
        .btn-action { background-color: #2196f3; }
        .btn-success { background-color: #4caf50; width: 100%; margin-top: 10px; }
        .btn-danger { background-color: #f44336; grid-column: span 2; margin-top: 5px; }
        
        .receipt-log {
            margin-top: 20px;
            font-size: 12px;
            color: #888;
            text-align: center;
            border-top: 1px dashed #444;
            padding-top: 10px;
        }
    </style>
</head>
<body>

    <div class="atm-container">
        <h2>★ SMART ATM ★</h2>
        
        <!-- Interactive Screen Output -->
        <div class="screen-display">
            <div id="screenText" class="screen-text">Welcome to the ATM.<br>Please select an option below.</div>
        </div>

        <!-- Dynamic Input Box for PIN and Amounts -->
        <div id="inputSection" class="input-group">
            <label id="inputLabel" for="atmInput">Enter Information:</label>
            <input type="number" id="atmInput" placeholder="0000" inputmode="numeric">
            <button id="submitBtn" class="button btn-success" onclick="processAction()">Confirm</button>
        </div>

        <!-- Core Grid Menu (Same layout as your C switch-case) -->
        <div id="menuGrid" class="grid-menu">
            <button class="btn-action" onclick="startTransaction(1)">Check Balance</button>
            <button class="btn-action" onclick="startTransaction(2)">Deposit Money</button>
            <button class="btn-action" onclick="startTransaction(3)">Withdraw Money</button>
            <button class="btn-danger" onclick="startTransaction(4)">Exit ATM</button>
        </div>

        <div id="receipt" class="receipt-log">System Status: Secure & Ready</div>
    </div>

    <script>
        // --- YOUR EXACT C VARIABLES ---
        let balance = 1000;
        const pin = 1234;
        let currentChoice = 0;

        // Elements tracking
        const screenText = document.getElementById("screenText");
        const inputSection = document.getElementById("inputSection");
        const inputLabel = document.getElementById("inputLabel");
        const atmInput = document.getElementById("atmInput");
        const menuGrid = document.getElementById("menuGrid");
        const receipt = document.getElementById("receipt");

        // Triggers when user clicks any main menu button
        function startTransaction(choice) {
            currentChoice = choice;
            atmInput.value = ""; // Clear old inputs

            if (choice === 4) {
                // Option 4: Exit
                screenText.innerHTML = "Thank you for visiting our ATM!<br>Session closed safely.";
                inputSection.style.display = "none";
                menuGrid.style.display = "none";
                receipt.innerHTML = "Transaction Log: Logged Out";
                return;
            }

            // Options 1, 2, and 3 all ask for a PIN verification first
            screenText.innerText = "Security Check:\nPlease verification PIN.";
            inputLabel.innerText = "Enter 4-Digit PIN:";
            atmInput.type = "password";
            atmInput.placeholder = "••••";
            inputSection.style.display = "block";
        }

        // Processes PIN verification or Amount entries
        function processAction() {
            const enteredValue = parseInt(atmInput.value);

            if (isNaN(enteredValue)) {
                screenText.innerHTML = "❌ Error:<br>Please enter a valid number.";
                return;
            }

            // STEP 1: Verify the Security PIN first
            if (atmInput.type === "password") {
                if (enteredValue !== pin) {
                    screenText.innerHTML = "❌ INVALID PIN!<br>Access Denied.";
                    inputSection.style.display = "none";
                    return;
                }
                
                // PIN verified! Proceed to your specific C switch-case logic
                executeMenuLogic();
            } else {
                // STEP 2: Handle Money Transactions
                handleMoneyTransaction(enteredValue);
            }
        }

        // Direct translation of your C Case structures
        function executeMenuLogic() {
            if (currentChoice === 1) {
                // Case 1: Check Balance
                screenText.innerHTML = `💳 Current Balance:<br><span style="color:#4caf50; font-size:24px;">$${balance}</span>`;
                inputSection.style.display = "none";
                receipt.innerHTML = "Transaction Log: Balance Inquiry Success";
            } 
            else if (currentChoice === 2) {
                // Case 2: Ask for Deposit Amount
                screenText.innerText = "Authorized.\nEnter deposit amount:";
                inputLabel.innerText = "Deposit Amount ($):";
                atmInput.type = "number";
                atmInput.placeholder = "0.00";
                atmInput.value = "";
            } 
            else if (currentChoice === 3) {
                // Case 3: Ask for Withdraw Amount
                screenText.innerText = "Authorized.\nEnter withdrawal amount:";
                inputLabel.innerText = "Withdrawal Amount ($):";
                atmInput.type = "number";
                atmInput.placeholder = "0.00";
                atmInput.value = "";
            }
        }

        // Handles conditional banking validation
        function handleMoneyTransaction(amount) {
            if (currentChoice === 2) {
                // Deposit conditional validation logic
                if (amount > 0) {
                    balance += amount;
                    screenText.innerHTML = `✅ Deposited Successfully!<br>New Balance: $${balance}`;
                    receipt.innerHTML = `Last Action: +$${amount} Deposited`;
                } else {
                    screenText.innerHTML = "❌ Fail to deposit.<br>Amount must be above 0.";
                }
            } 
            else if (currentChoice === 3) {
                // Withdrawal conditional validation logic
                if (amount > balance) {
                    screenText.innerHTML = "❌ Insufficient balance!<br>Transaction canceled.";
                } else if (amount <= 0) {
                    screenText.innerHTML = "❌ Invalid withdraw amount.<br>Try again.";
                } else {
                    balance -= amount;
                    screenText.innerHTML = `✅ Withdraw successful!<br>Remaining Balance: $${balance}`;
                    receipt.innerHTML = `Last Action: -$${amount} Withdrawn`;
                }
            }
            // Hide input prompt area after transaction completes
            inputSection.style.display = "none";
        }
    </script>
</body>
</html>
