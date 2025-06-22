# Firewall and IDS/IPS Evasion

Nmap gives us many different ways to bypass firewall rules and IDS/IPS. These methods include fragmentation of packets, the use of decoys, idle scans, and other evasive techniques.

---

## Firewalls

A **firewall** is a security mechanism that controls network traffic based on predetermined rules. It decides whether to allow or block specific traffic based on:

* Source/destination IP
* Ports
* Protocols

Firewalls prevent unauthorized access and can either **drop** (ignore) or **reject** (respond with error) suspicious packets.

---

## IDS/IPS

**Intrusion Detection Systems (IDS)** monitor network traffic for suspicious patterns and alert administrators.
**Intrusion Prevention Systems (IPS)** go a step further by taking actions to block potentially malicious traffic.

These systems often rely on **signature-based detection** and can be triggered by aggressive scans or known attack patterns.

---

## Detecting Firewalls with TCP ACK Scan

A **TCP ACK scan (-sA)** helps detect firewall behavior. Unlike SYN scans, ACK packets are often passed through firewalls.

Command:

```bash
sudo nmap -sA -p 21,22,25 10.129.2.28 -Pn -n --disable-arp-ping --packet-trace
```

If a port responds with **RST**, it is considered **unfiltered**. If there is no response, it is **filtered**.

---

## Comparing SYN and ACK Scans

SYN scan attempts a connection (3-way handshake), while ACK scan checks if a firewall allows ACK packets through.

* SYN responses: **SYN-ACK** = open, **RST** = closed
* ACK responses: **RST** = unfiltered, **No response** = filtered

---

## Detecting IDS/IPS

IDS/IPS detection is trickier. It involves:

* Scanning slowly to avoid alerts
* Using different VPS IPs to observe blocks
* Watching for disconnection or IP blacklisting

Aggressive scanning can alert IDS/IPS systems and lead to your IP being blocked.

---

## Decoy Scans (-D)

Decoy scans hide your real IP by spoofing several fake IPs.

```bash
sudo nmap -sS -p 80 10.129.2.28 -Pn -n --disable-arp-ping --packet-trace -D RND:5
```

This generates 5 random fake IPs. Your IP is randomly inserted among them.

### Important Notes:

* Decoy IPs should be alive or firewalls might detect SYN flood.
* Can be used with SYN, ACK, ICMP, and OS detection scans.

---

## Idle/Zombie Scan (-sI)

**Zombie scanning** uses a third-party idle host (zombie) to perform scans stealthily.

```bash
sudo nmap -sI <zombie_ip> <target_ip>
```

The zombie doesn’t interact with the target directly. Nmap uses it to send probe packets.

### Benefits:

* Completely masks the attacker's IP.
* Doesn’t trigger IDS easily.

### Requirements:

* Zombie host must have **predictable IP ID** sequence.
* Should not generate traffic on its own.

---

## What is IP ID?

The **IP ID** (Identification field) is a 16-bit number in IP headers used for packet fragmentation.

### Key Points:

* Every packet from a host has an incremented IP ID.
* If a host is idle, the IP ID increases only when packets are sent.
* Nmap leverages this behavior in idle scans to infer responses from the target.

---

## Firewall Rule Testing via OS Detection

To test if a firewall rule blocks access from specific IPs, try scanning using a different source IP.

```bash
sudo nmap 10.129.2.28 -n -Pn -p 445 -O -S 10.129.2.200 -e tun0
```

---

## DNS Proxying and Source Port Spoofing

* Nmap uses UDP port 53 for DNS queries by default.
* You can specify custom DNS servers using:

```bash
--dns-servers <ip1>,<ip2>
```

* Source port can be set to 53 to bypass firewall rules:

```bash
sudo nmap -sS -p 50000 10.129.2.28 -Pn -n --source-port 53
```

---

## Practical Test with Netcat

Try verifying open access by manually connecting:

```bash
ncat -nv --source-port 53 10.129.2.28 50000
```

If successful, it confirms that firewall rules allow traffic from TCP port 53.

---

## Summary

Firewalls and IDS/IPS can be bypassed with:

* Packet manipulation (ACK, decoys, idle scan)
* Source spoofing (source IP, source port)
* DNS proxying and stealth techniques

Use these techniques carefully and only in authorized environments.

---

## Lab Practice ⚔️

Apply the evasion techniques in real-world-like scenarios:

* Use ACK and idle scans to bypass filters
* Test source ports and decoys for scan stealth
* Identify if IPS blocks your IP after aggressive scans
