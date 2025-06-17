# 📘 Introduction to Nmap

**Network Mapper (Nmap)** is a powerful open-source network scanning and security auditing tool. It is written in **C, C++, Python, and Lua**, and is widely used to identify live hosts, detect open ports, determine services and their versions, discover operating systems, and even perform vulnerability assessments.

---

## 🛠️ Use Cases

Nmap is commonly used by network administrators, security professionals, and penetration testers to:

- 🔐 Audit the security of networks
- 🧪 Simulate penetration tests
- 🔥 Test firewall and IDS/IPS configurations
- 🗺️ Map out network topology
- 📡 Analyze host responses
- 🔍 Identify open and filtered ports
- ⚠️ Perform basic vulnerability assessments

---

## 🧱 Nmap Architecture

Nmap supports various scanning modules, each designed for a specific purpose:

- **Host Discovery** – Identifies live systems
- **Port Scanning** – Detects open/closed ports
- **Service Detection** – Identifies service names and versions
- **OS Detection** – Attempts to determine the operating system and version
- **Nmap Scripting Engine (NSE)** – Executes scripts to automate and extend functionality

---

## 💻 Nmap Syntax

Basic Nmap syntax is simple and flexible:

```bash
nmap <scan types> <options> <target>
````

---

## 🧪 Scan Techniques

Nmap provides a variety of scanning techniques depending on the information you're seeking. Use `nmap --help` to view them all:

```bash
bx00m3r@htb[/htb]$ nmap --help

<SNIP>
SCAN TECHNIQUES:
  -sS/sT/sA/sW/sM: TCP SYN/Connect()/ACK/Window/Maimon scans
  -sU: UDP Scan
  -sN/sF/sX: TCP Null, FIN, and Xmas scans
  --scanflags <flags>: Customize TCP scan flags
  -sI <zombie host[:probeport]>: Idle scan
  -sY/sZ: SCTP INIT/COOKIE-ECHO scans
  -sO: IP protocol scan
  -b <FTP relay host>: FTP bounce scan
<SNIP>
```

---

## 🧷 Example: TCP SYN Scan

The **TCP SYN scan (`-sS`)** is one of the fastest and most stealthy scan types. It doesn’t complete the TCP handshake, which can help it avoid detection in some cases.

### 🔁 Scan Logic:

* If the target replies with **SYN-ACK**, the port is **open**.
* If it responds with **RST**, the port is **closed**.
* If there's no response, it's likely **filtered** by a firewall.

### 📌 Example Command:

```bash
bx00m3r@htb[/htb]$ sudo nmap -sS localhost
```

### 📋 Sample Output:

```text
Starting Nmap 7.80 ( https://nmap.org ) at 2020-06-11 22:50 UTC
Nmap scan report for localhost (127.0.0.1)
Host is up (0.000010s latency).
Not shown: 996 closed ports
PORT     STATE SERVICE
22/tcp   open  ssh
80/tcp   open  http
5432/tcp open  postgresql
5901/tcp open  vnc-1

Nmap done: 1 IP address (1 host up) scanned in 0.18 seconds
```

Here we see four open TCP ports:

* **22**: SSH
* **80**: HTTP
* **5432**: PostgreSQL
* **5901**: VNC

This output helps us determine which services are accessible and potentially exploitable.

---
