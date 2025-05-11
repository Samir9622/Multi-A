<!DOCTYPE html>
<html>
<head>
    <title>Agency Launchpad</title>
    <link href="style.css" rel="stylesheet">
</head>
<body>
    <header>
        <h1>Agency Portal</h1>
        <div id="auth-section"></div>
    </header>
    
    <main>
        <section id="agency-grid"></section>
        <div id="notification-area"></div>
    </main>

    <script src="app.js"></script>
</body>
</html>
body {
    font-family: Arial, sans-serif;
    margin: 0;
    padding: 20px;
}

#agency-grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
    gap: 15px;
    margin-top: 20px;
}

.agency-card {
    border: 1px solid #ddd;
    padding: 15px;
    border-radius: 5px;
    cursor: pointer;
}

.agency-card:hover {
    background-color: #f5f5f5;
}

#auth-section {
    float: right;
}

button {
    padding: 8px 15px;
    margin-left: 10px;
    cursor: pointer;
}
// State management
let currentUser = null;
const agencies = [];

// DOM elements
const authSection = document.getElementById('auth-section');
const agencyGrid = document.getElementById('agency-grid');
const notificationArea = document.getElementById('notification-area');

// Initialize
loadAgencies();
renderAuthSection();

// Core functions
async function loadAgencies() {
    const response = await fetch('api/agencies.json');
    const data = await response.json();
    agencies.push(...data);
    renderAgencies();
}

function renderAuthSection() {
    authSection.innerHTML = currentUser 
        ? `<span>Welcome, ${currentUser.name}</span>
           <button onclick="logout()">Logout</button>`
        : `<button onclick="showLogin()">Login</button>
           <button onclick="showRegister()">Register</button>`;
}

function renderAgencies() {
    agencyGrid.innerHTML = agencies.map(agency => `
        <div class="agency-card" onclick="accessAgency('${agency.id}')">
            <h3>${agency.name}</h3>
            <p>${agency.description}</p>
        </div>
    `).join('');
}

function showNotification(message) {
    notificationArea.innerHTML = `<div class="notification">${message}</div>`;
    setTimeout(() => notificationArea.innerHTML = '', 3000);
}

// Action handlers
function accessAgency(agencyId) {
    if (!currentUser) {
        showNotification('Please login first');
        return;
    }
    showNotification(`Accessing ${agencyId}`);
    // Actual agency access logic here
}

function showLogin() {
    currentUser = { name: 'Test User' }; // Mock login
    renderAuthSection();
    showNotification('Logged in successfully');
}

function logout() {
    currentUser = null;
    renderAuthSection();
    showNotification('Logged out');
}

// Mock functions
function showRegister() {
    showNotification('Registration would open here');
}
[
    {
        "id": "finance",
        "name": "Finance Department",
        "description": "Budget and accounting services"
    },
    {
        "id": "hr",
        "name": "Human Resources",
        "description": "Staff management portal"
    },
    {
        "id": "operations",
        "name": "Operations",
        "description": "Daily business management"
    }
]
