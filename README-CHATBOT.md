# TrueScale AI Chatbot Documentation

This document explains the architecture, integration, and setup of the TrueScale AI Agent, a smart assistant integrated into the TrueScale Admin Portal. The chatbot allows administrators to manage the Smart Weight Tampering Detection System using natural language.

## Overview

The TrueScale AI Agent is a "tool-calling" AI powered by Google's Gemini API and LangChain. Instead of just answering questions, the chatbot can actually **take actions** and **read live data** from your system. 

When you ask it to "Show me recent tamper logs" or "Lockdown device006", the AI understands your intent, triggers the corresponding backend function, and reads the database to give you a natural language response based on real-time data.

---

## 1. System Architecture

The chatbot operates across three main layers:

### A. Frontend (The User Interface)
- **Files:** `dashboard.html`, `chatbot.css`, `chatbot.js`
- **How it works:** A floating chat widget is rendered on the dashboard. When an admin types a message, `chatbot.js` intercepts it, displays a "typing" indicator, and sends a `POST` request to the backend API (`http://localhost:3001/api/chat`).
- **Post-Action Triggers:** If the user sends a command like "lockdown" or "unlock", the frontend automatically triggers a dashboard refresh to show the updated device statuses immediately after the bot replies.

### B. Backend API (The Bridge)
- **Files:** `server.js`, `controllers/chatController.js`
- **How it works:** The Express backend receives the user's message and passes it to the `chatController.js`. This controller acts as the orchestrator between the user, the database, and the Google Gemini AI.
- **Environment:** The backend relies heavily on the `dotenv` package to securely load the `GOOGLE_API_KEY` from the `.env` file without exposing it in the source code.

### C. LangChain & Gemini (The Brains)
- **Library:** `@langchain/google-genai`
- **Model:** `gemini-3.5-flash`
- **How it works:** We use LangChain to bind our local Node.js functions (tools) to the Gemini model. When a message is sent to Gemini, we also send a description of all the "tools" it is allowed to use. Gemini decides if it needs to use a tool to answer the user's question.

---

## 2. How the AI "Tools" Work (Function Calling)

In `chatController.js`, we define `DynamicTool` objects. These are the superpowers given to the AI:

1. **`get_devices`**: Reads the MongoDB database (or fallback JSON) and returns a list of all devices and their statuses.
2. **`get_tamper_logs`**: Fetches the 50 most recent tampering events.
3. **`lockdown_device`**: Accepts a `deviceId`, updates that device's status to 'Tampered' in the database, and simulates sending an emergency lockdown command.
4. **`unlock_device`**: Accepts a `deviceId`, updates the status to 'Normal', and simulates an unlock command.

### The Request Lifecycle:
1. User types: *"Lockdown device006"*
2. Backend sends the message to Gemini along with the list of tools.
3. Gemini responds: *"I need to call the `lockdown_device` tool with the argument `deviceId: 'device006'`"*
4. The Node.js backend executes the `executeLockdown('device006')` function, which updates the database.
5. The backend sends the result ("Lockdown successful") back to Gemini.
6. Gemini reads the result and generates a final human-friendly response: *"Device006 has been successfully locked down."*
7. The response is sent back to the frontend and displayed in the chat window.

---

## 3. Setup and Configuration Requirements

To ensure the chatbot works correctly, the following configuration is strictly required:

### 1. API Key Configuration (`.env`)
The backend must have a `.env` file located at `smart-weight-backend/.env` containing a valid Google Gemini API Key:
```env
GOOGLE_API_KEY=AIzaSyAaw5zDrBRBQ3iXv...
```

### 2. Required NPM Packages
The `package.json` in the backend must include these specific LangChain and Google Generative AI packages:
```json
"dependencies": {
  "@langchain/core": "^1.1.48",
  "@langchain/google-genai": "^2.1.31",
  "dotenv": "^16.3.1",
  "zod": "^4.4.3"
}
```

### 3. Server Startup
Environment variables are loaded **only when the Node process starts**. 
- If you update the `.env` file or the `chatController.js` code, you **must** stop the backend server (`Ctrl + C`) and restart it using `npm start` or `npm run dev`.
- Ensure no lingering background Node processes are holding port `3001`.

---

## 4. Troubleshooting

- **Error: "GOOGLE_API_KEY is not configured"**
  - Check that the `.env` file exists inside the `smart-weight-backend` folder.
  - Verify that the variable is named exactly `GOOGLE_API_KEY`.
  - **Restart your Node server.**

- **Error: "404 Not Found ... models/..."**
  - Google frequently updates model names. If the API returns a 404 for a model string, update the `model` parameter inside `chatController.js` (e.g., changing from `gemini-1.5-pro` to `gemini-3.5-flash`). Note: The LangChain package now requires the property name to be `model` (not `modelName`).

- **Chatbot widget is not visible on the screen**
  - Ensure `chatbot.css` is linked in the `<head>` of your `dashboard.html`.
  - Ensure `chatbot.js` is linked at the bottom of the `<body>` of your `dashboard.html`.
  - Verify the HTML structure (`#chat-widget-btn` and `#chat-window`) exists in the `dashboard.html` file.