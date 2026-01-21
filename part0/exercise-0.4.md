<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Firebase Data Editor</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 20px;
            background-color: #f4f4f9;
        }
        h1 {
            color: #333;
            text-align: center;
        }
        .container {
            max-width: 1200px;
            margin: 0 auto;
            padding: 20px;
        }
        #data {
            background-color: #fff;
            padding: 20px;
            border-radius: 8px;
            box-shadow: 0 2px 10px rgba(0,0,0,0.1);
            min-height: 400px;
            max-height: 70vh;
            overflow: auto;
            margin: 20px 0;
            border: 1px solid #ddd;
            font-family: monospace;
            white-space: pre-wrap;
            word-wrap: break-word;
        }
        .controls {
            display: flex;
            justify-content: center;
            gap: 10px;
            margin-bottom: 20px;
        }
        button {
            background-color: #4285f4;
            color: white;
            border: none;
            padding: 10px 20px;
            border-radius: 4px;
            cursor: pointer;
            font-size: 16px;
            transition: background-color 0.3s;
        }
        button:hover {
            background-color: #3367d6;
        }
        button:disabled {
            background-color: #cccccc;
            cursor: not-allowed;
        }
        .status {
            text-align: center;
            margin: 10px 0;
            min-height: 24px;
            color: #0f9d58;
            font-weight: bold;
        }
        .error {
            color: #d32f2f;
        }
        .loading {
            text-align: center;
            padding: 20px;
            color: #666;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>Firebase Database Editor</h1>
        <div class="controls">
            <button id="saveBtn" disabled>Save Changes</button>
            <button id="refreshBtn">Refresh Data</button>
        </div>
        <div class="status" id="status"></div>
        <div id="data" contenteditable="true">Loading data...</div>
    </div>

    <script src="https://www.gstatic.com/firebasejs/9.22.1/firebase-app-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.22.1/firebase-database-compat.js"></script>
    <script>
        // Firebase configuration
        const firebaseConfig = {
            apiKey: "AIzaSyBYUKxfa_rwr4Tz8QMSFdv-fJAbUfukzOY",
            authDomain: "random-chat-c7141.firebaseapp.com",
            databaseURL: "https://random-chat-c7141-default-rtdb.asia-southeast1.firebasedatabase.app",
            projectId: "random-chat-c7141",
            storageBucket: "random-chat-c7141.firebasestorage.app",
            messagingSenderId: "355445365310",
            appId: "1:355445365310:web:746d9f396e2da1658bcf6f",
            measurementId: "G-Q2KQGZJ6JS"
        };

        // Initialize Firebase
        const app = firebase.initializeApp(firebaseConfig);
        const database = firebase.database();
        const dbRef = database.ref();

        // DOM elements
        const dataElement = document.getElementById('data');
        const saveBtn = document.getElementById('saveBtn');
        const refreshBtn = document.getElementById('refreshBtn');
        const statusElement = document.getElementById('status');

        // Load data from Firebase
        function loadData() {
            showStatus('Loading data...', 'loading');
            dbRef.once('value')
                .then((snapshot) => {
                    const data = snapshot.val();
                    dataElement.textContent = JSON.stringify(data, null, 2);
                    saveBtn.disabled = true;
                    showStatus('Data loaded successfully!', 'success');
                    // Clear the status after 3 seconds
                    setTimeout(clearStatus, 3000);
                })
                .catch((error) => {
                    showStatus('Error loading data: ' + error.message, 'error');
                });
        }

        // Save data to Firebase
        function saveData() {
            try {
                const newData = JSON.parse(dataElement.textContent);
                showStatus('Saving changes...', 'loading');
                dbRef.set(newData)
                    .then(() => {
                        showStatus('Changes saved successfully!', 'success');
                        saveBtn.disabled = true;
                        // Clear the status after 3 seconds
                        setTimeout(clearStatus, 3000);
                    })
                    .catch((error) => {
                        showStatus('Error saving data: ' + error.message, 'error');
                    });
            } catch (error) {
                showStatus('Invalid JSON: ' + error.message, 'error');
            }
        }

        // Show status message
        function showStatus(message, type = '') {
            statusElement.textContent = message;
            statusElement.className = 'status ' + type;
        }

        // Clear status message
        function clearStatus() {
            statusElement.textContent = '';
            statusElement.className = 'status';
        }

        // Event Listeners
        dataElement.addEventListener('input', () => {
            saveBtn.disabled = false;
        });

        saveBtn.addEventListener('click', saveData);
        refreshBtn.addEventListener('click', loadData);

        // Load data when page loads
        document.addEventListener('DOMContentLoaded', loadData);
    </script>
</body>
</html>
