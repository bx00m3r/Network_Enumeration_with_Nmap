# 🌐 Host Discovery with Nmap

When conducting an internal penetration test for an organization, the first step is to identify **which systems are online** and potentially accessible. This is known as **host discovery**. Nmap provides a range of methods for discovering live hosts within a network using various packet types such as ICMP, ARP, and more.

---

## 📋 Why Host Discovery Matters

Before targeting systems with detailed scans, we must know what’s *alive* on the network. Host discovery helps us:

- Get a quick overview of reachable devices
- Narrow down targets
- Avoid wasting time scanning inactive IPs
- Document and store results for future reference

💡 **Pro Tip:** Always save your scan results. Different tools and methods may yield different outcomes — logging results helps with comparison and reporting later.

---

## 🌍 Scan a Network Range

```bash
sudo nmap 10.129.2.0/24 -sn -oA tnet | grep for | cut -d" " -f5
````

### 🧾 Explanation

| Option          | Description                                            |
| --------------- | ------------------------------------------------------ |
| `10.129.2.0/24` | Target network range                                   |
| `-sn`           | Ping scan (disable port scanning)                      |
| `-oA tnet`      | Save output in all formats (`tnet.nmap`, `.xml`, etc.) |

This works only if hosts respond to ICMP Echo requests or ARP probes. If firewalls block pings, use alternate techniques (covered in the "Firewall and IDS Evasion" section).

---

## 📁 Scan from an IP List

```bash
cat hosts.lst
```

```
10.129.2.4  
10.129.2.10  
10.129.2.11  
10.129.2.18  
10.129.2.19  
10.129.2.20  
10.129.2.28  
```

```bash
sudo nmap -sn -oA tnet -iL hosts.lst | grep for | cut -d" " -f5
```

| Option | Description                       |
| ------ | --------------------------------- |
| `-iL`  | Load target IPs from `hosts.lst`  |
| `-sn`  | Disable port scanning (ping-only) |
| `-oA`  | Save output in all formats        |

This shows which hosts in the list are currently alive.

---

## 📌 Scan Multiple IPs

```bash
sudo nmap -sn -oA tnet 10.129.2.18 10.129.2.19 10.129.2.20
```

Or using a range shortcut:

```bash
sudo nmap -sn -oA tnet 10.129.2.18-20
```

---

## 🎯 Scan a Single IP

```bash
sudo nmap 10.129.2.18 -sn -oA host
```

### Sample Output

```text
Nmap scan report for 10.129.2.18  
Host is up (0.087s latency).  
MAC Address: DE:AD:00:00:BE:EF  
```

| Option     | Description                             |
| ---------- | --------------------------------------- |
| `-sn`      | Disable port scan (ping only)           |
| `-oA host` | Output all formats starting with `host` |

Nmap uses **ARP ping** by default on local networks — if a device replies to ARP, it's marked alive.

---

## 🧪 Forcing ICMP Echo Scan

```bash
sudo nmap 10.129.2.18 -sn -oA host -PE --packet-trace
```

### Output

```text
SENT ARP who-has ...  
RCVD ARP reply ...  
```

| Option           | Description                            |
| ---------------- | -------------------------------------- |
| `-PE`            | Use ICMP Echo Request                  |
| `--packet-trace` | Show all packets sent/received by Nmap |

By default, ARP is used on local subnets. To force only ICMP:

---

## 🔍 Disable ARP and Use Only ICMP

```bash
sudo nmap 10.129.2.18 -sn -oA host -PE --packet-trace --disable-arp-ping
```

This disables ARP and ensures only ICMP packets are used:

```text
SENT ICMP Echo request  
RCVD ICMP Echo reply  
```

---

## 🧠 Get Reason for Host Status

```bash
sudo nmap 10.129.2.18 -sn -oA host -PE --reason
```

Output:

```text
Host is up, received arp-response
```

| Option     | Description                           |
| ---------- | ------------------------------------- |
| `--reason` | Explains why a host is marked as "up" |

Nmap clearly shows how it determined the host was alive (e.g., ARP, ICMP, etc.).

---

## 📚 More on Host Discovery

Nmap provides detailed documentation on host discovery strategies. Explore more here:
🔗 [https://nmap.org/book/host-discovery-strategies.html](https://nmap.org/book/host-discovery-strategies.html)

---

## ✅ Challenge: OS Identification via ICMP

From the final packet-traced ICMP response:

> Based on the TTL and reply behavior, the OS is likely:

🖥️ **Answer**: `Windows`

---
