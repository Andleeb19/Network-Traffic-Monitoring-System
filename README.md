# 🛰️ Network Traffic Monitoring System


## 📌 Introduction

This project implements a **real-time network traffic monitoring system** that captures, analyzes, and visualizes network metrics using a modular architecture. It is built with C++ for backend packet capture, SQLite for data storage, and a Flask + Chart.js-based dashboard for visualization.

---

## 🎯 Project Objectives

- Monitor real-time network traffic.
- Analyze traffic by protocol (TCP/UDP/Other).
- Store metrics efficiently using SQLite.
- Display metrics through a responsive web dashboard.

---

## 🧰 System Requirements

- Windows OS with [Npcap](https://npcap.com/) installed
- C++ compiler (e.g., MinGW)
- Python 3.x
- Flask (`pip install flask`)
- SQLite3

---

## 🧱 System Architecture

+--------------------------+
| Packet Capture Engine | <-- C++ using WinPcap/Npcap
| - Captures traffic |
| - Parses IP headers |
| - Calculates metrics |
+-----------+--------------+
|
v
+--------------------------+
| Data Storage Layer | <-- SQLite Database
| - Stores metrics |
| - Ensures thread safety |
+-----------+--------------+
|
v
+--------------------------+
| Web Visualization (Flask)| <-- Python Flask + HTML/CSS/JS
| - REST API endpoints |
| - Dashboard via Chart.js|
+--------------------------+


---

## ⚙️ Components

### 🧪 Packet Capture Engine (C++)

- Captures packets in real-time using **WinPcap/Npcap**
- Extracts key metrics: IPs, ports, protocols, byte size
- Data structures used:

```cpp
struct ProtocolMetrics {
    int tcp_count = 0;
    int udp_count = 0;
    int other_count = 0;
    std::string hostname;
    int bytes_transferred = 0;
};

struct ConnectionMetrics {
    int packet_count = 0;
    ProtocolMetrics protocol_metrics;
    float bytes_per_second = 0.0;
};
Periodically saves metrics to SQLite using store_metrics() and periodic_store_metrics() functions

💾 Data Storage (SQLite)
Lightweight, file-based DB

Thread-safe writes with retry logic

Schema includes:

Connection IPs

Packet/byte counts

Transfer rates

Protocol types

🌐 Web Dashboard (Flask + HTML/CSS/JS)
Flask provides backend API via /metrics and /status

Real-time dashboard includes:

📊 Bar chart for traffic per connection

🍩 Doughnut chart for protocol distribution

📋 Active connections table

Built with Chart.js, refreshed every second via AJAX

Example Python API handler:

python
Copy
Edit
def fetch_metrics():
    try:
        with sqlite3.connect(DB_PATH) as conn:
            conn.row_factory = sqlite3.Row
            cursor = conn.cursor()
            # Fetch and process metrics
            return processed_metrics
    except Exception as e:
        log_error(e)
        return default_metrics
🚀 How to Run
🛠️ Compile C++ Packet Capture
bash
Copy
Edit
# Open terminal or use IDE task (e.g., Ctrl+Shift+B in VS Code)
g++ -o capture_packets.exe capture.cpp -lws2_32 -lPacket
🌍 Run Flask Web Server
bash
Copy
Edit
# From project root
python app.py
Then open http://127.0.0.1:5000 in your browser.

🖥️ Dashboard Overview
Stat Boxes: Show active connections, total packets, and bytes transferred

Bar Chart: Live traffic per IP/connection

Doughnut Chart: Distribution of TCP, UDP, and Other protocols

Table: Per-connection details (auto-refresh every second)

✅ Key Achievements
Real-time packet capture and protocol classification

Reliable storage using SQLite

Smooth, responsive web dashboard for visualization

Structured multi-language integration (C++ + Python + JS)
