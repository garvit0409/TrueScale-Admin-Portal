# TrueScale Admin Portal - Smart Weight Tampering Detection System

![TrueScale Admin Portal](https://img.shields.io/badge/TrueScale-Admin%20Portal-6C63FF?style=for-the-badge)
![Node.js](https://img.shields.io/badge/Node.js-339933?style=for-the-badge&logo=nodedotjs&logoColor=white)
![Express.js](https://img.shields.io/badge/Express.js-000000?style=for-the-badge&logo=express&logoColor=white)
![MongoDB](https://img.shields.io/badge/MongoDB-4EA94B?style=for-the-badge&logo=mongodb&logoColor=white)
![Vanilla JS](https://img.shields.io/badge/Vanilla_JS-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black)
![Python ML](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)
![Gemini AI](https://img.shields.io/badge/Gemini_AI-8E75B2?style=for-the-badge&logo=google&logoColor=white)

The **TrueScale Admin Portal** is a comprehensive, full-stack IoT administration platform designed for monitoring, managing, and securing smart weighing scales. It leverages advanced Machine Learning (Python) to detect tampering, Node.js + MongoDB for robust real-time tracking, and a Google Gemini-powered AI chatbot for executing natural language commands on hardware devices.

---

## 🚀 Key Features

*   **Real-time Dashboard**: Interactive GPS maps (Leaflet.js) and Analytics (Chart.js) to monitor all devices across geographical locations.
*   **Machine Learning Tamper Detection**: Python-based anomaly detection models that monitor scale readings and raise immediate alerts on suspected tampering or unauthorized weight changes.
*   **AI Agent (Gemini & LangChain)**: An integrated "tool-calling" AI chatbot that allows admins to execute system actions via natural language (e.g., *"Lockdown device006"* or *"Show recent tamper logs"*).
*   **Device Request Management System**: Automated flow for merchants to request scale devices, complete with admin approvals, automated User/Account creation, and automatic PDF Certificate Generation (via `PDFKit`).
*   **Dynamic Data Sources**: Uses **MongoDB Atlas** as the primary datastore, seamlessly falling back to local JSON stores if the cloud database is unavailable.

---

## 🏗️ System Architecture

The ecosystem consists of three main pillars:

### 1. Frontend (`/frontend`)
*   **Tech Stack**: Vanilla HTML5, CSS3, JavaScript (ES6+), Leaflet.js, Chart.js.
*   **Functionality**: Serves the UI for the main Admin Dashboard, interactive Maps, Admin Device/Request management panels, and the floating AI Chatbot widget.

### 2. Backend API (`/smart-weight-backend`)
*   **Tech Stack**: Node.js, Express.js, MongoDB (Mongoose), `dotenv`, `bcrypt`.
*   **Functionality**: Exposes RESTful endpoints for CRUD operations (devices, logs, analytics, user requests). Bridges the frontend UI with the database and ML anomaly detection services. Generates PDF certificates for approved devices.

### 3. AI & ML Integrations
*   **Generative AI**: Uses `@langchain/google-genai` bound to Node.js backend controllers to convert natural admin chats into actionable backend commands (Dynamic Tools).
*   **Tampering Detection**: Python ML environment (`/smart-weight-backend/ml`) containing prediction scripts (`exp_detector.py`) that simulate/detect scale discrepancies. 

---

## 📂 Directory Structure

```text
TrueScale_AdminPortal/
├── frontend/                     # Client-side UI application
│   ├── assets/                   # CSS and JS files (admin.js, chatbot.js, etc.)
│   ├── dashboard.html            # Main dashboard interface
│   ├── admin.html                # Device request & admin panel
│   └── index.html                # Landing page
│
├── smart-weight-backend/         # Server-side API & ML services
│   ├── certificates/             # Auto-generated device PDF certificates
│   ├── controllers/              # Business logic (e.g., chatController.js)
│   ├── data/                     # Fallback JSON datastore & ML dataset (weights.csv)
│   ├── ml/                       # Python machine learning tampering scripts
│   ├── models/                   # MongoDB Mongoose schemas (DeviceRequest.js, User.js)
│   ├── server.js                 # Main Express server entry point
│   └── import_csv_to_mongodb.js  # Script to populate fresh MongoDB DB
│
└── start-server.sh               # Quick startup shell script
```

---

## ⚙️ Setup & Installation

### Prerequisites
*   **Node.js** (v14+ recommended)
*   **Python** (v3.8+)
*   **MongoDB Atlas** account (or local MongoDB server)
*   **Google Gemini API Key** (for chatbot functionality)

### 1. Backend Setup

1.  **Navigate to the backend directory:**
    ```bash
    cd smart-weight-backend
    ```
2.  **Install Node dependencies:**
    ```bash
    npm install
    ```
3.  **Environment Variables (`.env`):**
    Create a `.env` file in `smart-weight-backend/` and include:
    ```env
    PORT=3001
    MONGODB_URI=your_mongodb_connection_string
    DB_NAME=smart_weight_detection
    GOOGLE_API_KEY=your_gemini_api_key
    NODE_ENV=development
    ```
4.  **Database Seeding (First-time setup):**
    If your MongoDB is empty, seed it with the provided CSV/JSON data:
    ```bash
    node import_csv_to_mongodb.js
    ```
5.  **Start the API Server:**
    ```bash
    npm start
    # or npm run dev for nodemon
    ```
    *The server runs on `http://localhost:3001`.*

### 2. Frontend Setup

1.  **Navigate to the frontend directory:**
    ```bash
    cd frontend
    ```
2.  **Install dependencies (live-server):**
    ```bash
    npm install
    ```
3.  **Start Frontend Server:**
    ```bash
    npm start
    ```
    *The frontend typically runs on `http://localhost:8080` (or whichever port live-server binds to).*

> 💡 **Tip:** You can use the `start-server.sh` wrapper script in the root directory to simplify booting up the backend environments!

---

## 🛠️ Usage Flow

1.  **Dashboard Monitoring**: Open `http://localhost:3001/dashboard.html` (or your frontend dev server port) to view live maps and tamper metrics.
2.  **Device Setup Requests**: Vendors visit the login page and fill out a "Request New Device" form. Admins approve this from the dashboard, generating a new Device ID, Account Credentials, and a stamped PDF Certificate saved to `certificates/`.
3.  **AI Command Chatbot**: Click the chat widget in the corner of the dashboard. Ask it things like:
    *   *"How many devices are currently active?"*
    *   *"Show me the 5 most recent tamper logs."*
    *   *"Lockdown device006"* (Triggers a secure backend command updating MongoDB status to 'Tampered' and refreshing the map UI).
4.  **Machine Learning Simulations**: Python scripts in the `/ml` directory routinely test incoming weight configurations against expected variances, writing anomaly findings back to the server logs.

---

## 📄 Documentation Links
For deeper dives into individual system components, refer to the detailed READMEs provided within the repository:
*   [MongoDB Setup & Status](./MONGODB-SETUP.md)
*   [Backend API Specs](./smart-weight-backend/README.md)
*   [Frontend UI Specs](./frontend/README.md)
*   [AI Chatbot & Tool Calling Architecture](./README-CHATBOT.md)
*   [Device Requests & Certificates](./README-DEVICE-REQUESTS.md)

---
*TrueScale Smart Weight System — Secure, Monitor, & Manage.*
