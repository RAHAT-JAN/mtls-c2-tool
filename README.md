# mTLS C2 Tool – Secure Command and Control with GUI Dashboard  

**Repository Description:**  
Guide and implementation of a secure **mutual TLS (mTLS) Command-and-Control (C2) system** with GUI dashboard, supporting encrypted communication, file transfer, root command execution, and real-time analytics.  

**GitHub Topics / Tags:**  
`python` `mtls` `c2-framework` `red-team` `cybersecurity` `tls` `encryption` `pyqt5` `command-and-control`  


##  Overview  
This project implements a **secure, full-featured Command-and-Control (C2) system** using **mutual TLS (mTLS)** for encrypted communication.  
It simulates real-world **red team infrastructure** and **remote support environments**, providing a modern **GUI-based dashboard** for remote administration of client machines.  

The system supports:  
- Root-level command execution  
- Secure file transfer  
- Session logging  
- Real-time analytics dashboards  

This makes it both a practical research project and a learning tool for **secure communications** and **red-team tooling**.  


##  Key Features  
- 🔒 **Encrypted Communication** – Full mTLS with X.509 certificates and AES-encrypted payloads  
- 🖥 **GUI Dashboard** – Built with PyQt5, supports multi-client tabs, throughput visualization, and command history  
- ⚡ **Root Access** – Commands run with `sudo` on the client without password prompts  
- 📂 **File Transfer** – Secure upload/download using base64 over TLS  
- 📝 **Session Logging** – All interactions logged with timestamps for audit and forensics  
- 📊 **Analytics Dashboard** – Command usage statistics visualized in real time  


## Architecture  

### Server (Kali Linux 2023.1, Python 3.13)  
- GUI dashboard (PyQt5, matplotlib, pyqtgraph)  
- Listens for incoming TLS connections  
- Each client has a dedicated tab for interaction  
- Displays command history, throughput, and statistics  

### Client (Ubuntu 24.04)  
- Connects securely using TLS certificates  
- Executes shell commands, transfers files, captures screenshots  
- Runs with elevated privileges (via sudoers config)  


##  Certificate Setup  

This project uses **custom CA and certificates** generated with OpenSSL.  

```bash
# 1. Create Root CA
openssl genrsa -out ca.key 4096
openssl req -x509 -new -nodes -key ca.key -sha256 -days 365   -subj "/CN=Kali-RootCA" -out ca.crt

# 2. Server Certificate
openssl req -newkey rsa:2048 -nodes -keyout server.key   -subj "/CN=KaliServer" -out server.csr
openssl x509 -req -in server.csr -CA ca.crt -CAkey ca.key   -CAcreateserial -out server.crt -days 365

# 3. Client Certificate
openssl req -newkey rsa:2048 -nodes -keyout client.key   -subj "/CN=UbuntuAgent" -out client.csr
openssl x509 -req -in client.csr -CA ca.crt -CAkey ca.key   -CAcreateserial -out client.crt -days 365
```

👉 Copy `client.crt`, `client.key`, and `ca.crt` to the **client machine** (`/etc/rat/certs`).  


## Root Access Without Prompt  

To allow commands to run with root privileges without a password prompt, add this on the **client (Ubuntu)**:  

```bash
echo "ubuntu ALL=(ALL) NOPASSWD: ALL" | sudo tee /etc/sudoers.d/ubuntu
sudo chmod 0440 /etc/sudoers.d/ubuntu
```

Now, any command sent with `sudo` executes directly as root.  


## ▶️ How to Run  

### On the Server (Kali Linux)  
```bash
python3 mtls_c2_gui_dash.py
```

### On the Client (Ubuntu)  
```bash
python3 client.py
```

⚠️ Make sure both machines have the certificates in the `certs/` directory and that IP addresses are correctly set in the code.  

---

##  Demonstrated Capabilities  

### 1. File Transfer  
```bash
put /root/test.txt /tmp/test.txt
# Output: [+] Saved /tmp/test.txt
```

### 2. Root Command Execution  
```bash
sudo whoami
# Output: root
```

### 3. Session Logging  
- Each command logged with timestamp and client identity  
- Stored in `session.log`  

### 4. TLS Encryption Validation  
- Verified with Wireshark → TLSv1.2 Application Data packets confirm encryption  

### 5. Usage Analytics  
- Dashboard displays bar chart of command frequency for usability insights  

---

##  Suggested Repository Structure  

```
mtls-c2-tool/
│
├── README.md                   # Complete project documentation
├── server/
│   └── mtls_c2_gui_dash.py      # Server-side code
├── client/
│   └── client.py                # Client-side code
├── certs/                       # Certificates (server, client, CA)
├── docs/
│   └── task.pdf                 # Detailed documentation (optional)
└── LICENSE
```


## 📖 References  
- [PyQt5 Documentation](https://pypi.org/project/PyQt5/)  
- [Mutual TLS Overview](https://www.cloudflare.com/learning/access-management/what-is-mutual-tls/)  
- [Matplotlib Documentation](https://matplotlib.org/stable/index.html)  
- [PyQtGraph Documentation](http://www.pyqtgraph.org/)  


## ⚠️ Disclaimer  
This project is intended for **educational and research purposes only**.  
- Do **NOT** use it on unauthorized systems.  
- The author is not responsible for misuse.  


## 📜 License  
This project is licensed under the **MIT License** – you are free to use, modify, and distribute it.  
