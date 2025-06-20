# Service Enumeration

Service enumeration is a crucial step in network reconnaissance. It helps us identify the services running on open ports and extract additional details such as version numbers, configurations, and sometimes even underlying operating systems. This information is vital for identifying potential vulnerabilities and determining the attack surface.

---

## Service Version Detection

It is generally recommended to perform a **quick port scan first** to get an overview of open ports. This approach generates less network traffic, making it less likely to be detected and blocked by security systems.

Once we have a list of open ports, we can use **Nmap’s service version detection** feature to identify the exact service and version running on those ports.

Command:

```bash
sudo nmap 10.129.2.28 -p- -sV
```

Sample Output:

```bash
Starting Nmap 7.80 ( https://nmap.org )
PORT      STATE    SERVICE      VERSION
22/tcp    open     ssh          OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
25/tcp    open     smtp         Postfix smtpd
80/tcp    open     http         Apache httpd 2.4.29 ((Ubuntu))
...
```

### Scanning Options

| Option        | Description                                          |
| ------------- | ---------------------------------------------------- |
| `10.129.2.28` | Scans the specified target IP address.               |
| `-p-`         | Scans **all ports (1-65535)**.                       |
| `-sV`         | Enables version detection of services on open ports. |

---

### Viewing Scan Status

You can press the **\[Space Bar]** during an Nmap scan to check its status in real-time.

Alternatively, you can use the `--stats-every` option:

```bash
sudo nmap 10.129.2.28 -p- -sV --stats-every=5s
```

This shows the scan progress every 5 seconds.

---

### Verbosity Option

Increase verbosity to display real-time results during scanning:

```bash
sudo nmap 10.129.2.28 -p- -sV -v
```

Use `-vv` for even more verbose output.

---

## Banner Grabbing

Nmap attempts to identify service versions through banner grabbing. If a service reveals identifying information upon connection, Nmap will capture and display it.

However, **manual banner grabbing** can often reveal more details than automated tools.

Use `nc` (netcat) to manually connect to a service:

```bash
nc -nv 10.129.2.28 25
```

Output:

```bash
Connection to 10.129.2.28 port 25 [tcp/*] succeeded!
220 inlane ESMTP Postfix (Ubuntu)
```

You can also capture this exchange using `tcpdump`:

```bash
sudo tcpdump -i eth0 host 10.10.14.2 and 10.129.2.28
```

---

## Detailed Packet Exchange (Example)

Using tcpdump, we can see the TCP handshake and data exchange:

```
1. SYN      > [S] Flag from 10.10.14.2 to 10.129.2.28:25
2. SYN-ACK  > [S.] Flag from 10.129.2.28 to 10.10.14.2
3. ACK      > [.] Flag from 10.10.14.2 to 10.129.2.28
4. PSH-ACK  > [P.] Flag (Server sends banner)
5. ACK      > [.] Flag (Client acknowledges)
```

This demonstrates that services often provide data **immediately after a successful handshake** using a PSH (Push) flag.

---

## Advanced Scan Options

Sometimes we want to perform stealthier or more detailed scans:

```bash
sudo nmap 10.129.2.28 -p- -sV -Pn -n --disable-arp-ping --packet-trace
```

### Explanation:

| Option               | Description                                |
| -------------------- | ------------------------------------------ |
| `-Pn`                | Skips host discovery (treats host as up).  |
| `-n`                 | Disables DNS resolution.                   |
| `--disable-arp-ping` | Skips ARP ping requests.                   |
| `--packet-trace`     | Displays sent/received packet information. |

---

### Try It Yourself in HTB Lab ⚔️

```bash
# Task 1: Run a full TCP port scan with version detection
sudo nmap -p- -sV 10.129.2.28

# Task 2: Manually grab SMTP banner
nc -nv 10.129.2.28 25

# Task 3: Observe packet exchange with tcpdump
sudo tcpdump -i eth0 host 10.10.14.2 and 10.129.2.28
```

---

### Related Questions ✅

Perform in Module Specific Lab for better understanding!

Q) Enumerate all ports and their services. One of the services contains the flag you have to submit as the answer.
`Answer: `

---

This concludes service enumeration and banner grabbing using Nmap. Continue exploring by digging deeper into open services and potential vulnerabilities!
