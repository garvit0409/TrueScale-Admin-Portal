TrustScale: Autonomous AI-Powered Cyber-Physical Security Ecosystem
TrustScale is an end-to-end security ecosystem designed to protect digital weighing infrastructures from physical tampering and calibration fraud. By bridging custom embedded hardware, native mobile applications, a centralized analytics dashboard, and an autonomous AI agent workflow, the system intercepts security threats in real-time and executes instant lockdown protocols.

🏗️ Repository Ecosystem Hub
This project is split across specialized repositories for modular deployment.

Central Control Panel: TrueScale Admin Portal (Current Repository) — Streamlit dashboard, SQLite logging, and LangChain configuration.

Embedded Controller: TrustScale AI Security Firmware — ESP32 microcontroller code and sensor telemetry.

User Application: TrustScale User App — Native Kotlin application for regular weight monitoring.

Auditor Application: TrustScale Auditor App — Native Kotlin application for compliance tracking and alert reviews.

📷 System Architecture & Evidence of Work
1. End-to-End Data Flow Diagram
    [ ESP32 Hardware Module ] 
               |
     (Bluetooth Telemetry / Pin Interrupts)
               |
               v
    [ Native Android Apps ] --(Retrofit REST APIs)--> [ Streamlit Central Control Panel ]
     (User & Auditor Hub)                                      |
                                                       (Telemetry Analysis)
                                                               |
                                                               v
    [ SQLite Secure Logs ] <======================= [ LangChain / Gemini AI Agent ]
                                                    - Automated Threat Analysis
                                                    - Tool Calling & DB Queries
                                                    - Real-Time System Lockdown
💡 Recruiter Note / Project Proof:

Hardware Proof: ([url](https://www.linkedin.com/posts/contact-garvit-arora_smartindiahackathon-sih-aicte-ugcPost-7408480922867564544-uGDD/))

Software Proof: ([url](https://drive.google.com/file/d/1LCIL1fH8BrB-Bw7LZdqynDwDOMSkz_I8/view))

🛠️ Detailed Layer Breakdown
1. Embedded Hardware & Signal Engineering (firmware/)
The physical hardware module intercepts data and physical breaches right at the machine level using an ESP32 microcontroller.

RS232 Interception: Handled weight scale communication data streams using a MAX232 interface IC.

Signal Tamper Tracking: Designed hardware logic monitoring the M+ calibration lines to instantly catch voltage drops or spikes indicating fraudulent manipulation.

Physical Case Defense: Integrated an ultrasonic distance sensor and a push-button array to sense if the physical chassis structure is opened or compromised.

Local Anomaly Detection: Scripts stress-tested and caught low-voltage drops (3.3V dropping to 0.1V for over 3000ms), immediately flagging them as critical tampering events.

2. Autonomous AI Workflows & Control Panel (Admin Portal)
The command center processes telemetry data and acts automatically using Generative AI workflows.

LangChain Agent Core: Built using the Gemini API. The agent doesn't just read data; it uses Tool Calling to directly query the local database when an anomaly is detected.

Autonomous Incident Response: If telemetry flags an ongoing attack, the agent evaluates the threat score and independently initiates system lockdown workflows.

Analytics Dashboard: Powered by Streamlit to display live device state histories and store immutable system logs via a secure SQLite pipeline.

3. Mobile Ecosystem (User App & Auditor App)
Two native Android applications developed in Kotlin act as the primary interface layers for field operations.

Networking: Utilizes Retrofit2 to maintain lightweight, reliable API communication pipelines with the central backend architecture.

Telemetry Handling: Decodes data streams and allows authorized administrators to review physical system statuses and reset lockdown states after inspection.

🚀 Technical Stack Summary
Firmware: C++, Arduino Core Framework, ESP32 BLE/Serial libraries.

Backend & AI Agent: Python, LangChain, Gemini API, Streamlit Framework.

Database: SQLite.

Mobile Apps: Kotlin, Jetpack Libraries, Retrofit2.

🔧 Local Configuration & Setup
To spin up the Central Control Panel and AI Environment locally:

1. Clone the Central Repository
Bash
git clone https://github.com/garvit0409/TrueScale-Admin-Portal.git
cd TrueScale-Admin-Portal
2. Install Dependencies
Bash
pip install -r requirements.txt
3. Set Up Environment Keys
Create a .env file in the root directory:

Code snippet
GEMINI_API_KEY=your_secure_gemini_api_key_here
DATABASE_URL=sqlite:///security_logs.db
4. Run the Control Dashboard
Bash
streamlit run app.py
