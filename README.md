# 🔐 mTLS C2 Tool – Secure Remote Access Dashboard

## 📦 Features
- Mutual TLS-authenticated connection (X.509 certs)
- Full GUI with multi-client dashboards
- Command execution with root privileges (NOPASSWD)
- Secure file transfer (upload/download)
- Real-time throughput and stats visualization
- Logging of all session activity

---

## 🚀 Quick Start

### 🖥 Server (Kali)
```bash
cd server
./install-deps.sh
python3 mtls_c2_gui_dash.py
💻 Client (Ubuntu)
cd client
./install-deps.sh
python3 client.py
