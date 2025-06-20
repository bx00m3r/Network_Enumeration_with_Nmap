# 🌐 Host and Port Scanning with Nmap

Understanding how Nmap works internally is crucial for interpreting its results accurately. Once we've discovered that a host is online, our next step is to probe its services, ports, and underlying systems. This section explores scanning techniques and how to analyze the results.

---

## 🔎 What We Aim to Discover

* Open ports and services
* Service versions
* Service banners/info
* Operating system (OS) fingerprinting

---

## 📊 Port States Explained

| State        | Description                                                   |                                                          
| ------------ | ------------------------------------------------------------- |
| `open`       | A service is listening on the port                            |                                                          
| `closed`     | No service is listening (RST response received)               |                                                          
| `filtered`   | Nmap can't determine (no response or error returned)          |                                                          
| `unfiltered` | Nmap can reach the port but can't tell if it's open or closed |                                                          
| `open\|filtered` | Nmap can't decide if the port is open or filtered        |
| `closed\|filtered` | Occurs in IP ID idle scans; unsure if closed or filtered |

---

## 🚀 TCP SYN Scan (`-sS`)

```bash
sudo nmap 10.129.2.28 --top-ports=10
```

This command scans the **top 10 most common TCP ports**. It requires root privileges to perform a SYN scan.

### Example Output:

```
PORT     STATE    SERVICE
21/tcp   closed   ftp
22/tcp   open     ssh
25/tcp   open     smtp
80/tcp   open     http
110/tcp  open     pop3
139/tcp  filtered netbios-ssn
445/tcp  filtered microsoft-ds
```

To observe raw packets:

```bash
sudo nmap 10.129.2.28 -p 21 --packet-trace -Pn -n --disable-arp-ping
```

---

## 🛡️ TCP Connect Scan (`-sT`)

This method completes the full TCP 3-way handshake. It's slower and more detectable but accurate.

```bash
sudo nmap 10.129.2.28 -p 443 -sT --packet-trace --disable-arp-ping -Pn -n --reason
```

### Example:

```
PORT    STATE SERVICE REASON
443/tcp open  https   syn-ack
```

---

## ⛔️ Filtered Ports

When a firewall **drops packets**, Nmap keeps retrying (up to 10 times by default):

```bash
sudo nmap 10.129.2.28 -p 139 --packet-trace -n --disable-arp-ping -Pn
```

When a firewall **rejects packets**, Nmap might receive an ICMP error:

```bash
sudo nmap 10.129.2.28 -p 445 --packet-trace -n --disable-arp-ping -Pn
```

Result:

```
PORT    STATE    SERVICE
445/tcp filtered microsoft-ds
```

---

## 💡 UDP Scanning (`-sU`)

UDP is stateless, making scans slower and less predictable.

```bash
sudo nmap 10.129.2.28 -F -sU
```

Example:

```
PORT     STATE         SERVICE
68/udp   open|filtered dhcpc
137/udp  open          netbios-ns
138/udp  open|filtered netbios-dgm
```

To get packet-level insight:

```bash
sudo nmap 10.129.2.28 -sU -Pn -n --disable-arp-ping --packet-trace -p 137 --reason
```

Or observe a **closed** port:

```bash
sudo nmap 10.129.2.28 -sU -Pn -n --disable-arp-ping --packet-trace -p 100 --reason
```

Or a port marked `open|filtered`:

```bash
sudo nmap 10.129.2.28 -sU -Pn -n --disable-arp-ping --packet-trace -p 138 --reason
```

---

## 📃 Version Detection (`-sV`)

```bash
sudo nmap 10.129.2.28 -Pn -n --disable-arp-ping --packet-trace -p 445 --reason -sV
```

### Example:

```
PORT    STATE SERVICE     VERSION
445/tcp open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
```

Nmap uses various service probes to fingerprint running versions, including `NULL`, `SMBProgNeg`, etc.

---

## 📖 Learn More

Read more about advanced port scanning techniques in the official Nmap documentation:

➡️ [https://nmap.org/book/man-port-scanning-techniques.html](https://nmap.org/book/man-port-scanning-techniques.html)
