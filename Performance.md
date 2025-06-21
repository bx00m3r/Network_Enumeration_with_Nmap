# Performance

Scanning performance plays a significant role when we need to scan a large network or operate in environments with limited bandwidth. Nmap provides a range of options to optimize performance, including tuning timeouts, retry counts, scan rates, and predefined timing templates.

---

## Timeouts and Round-Trip Time (RTT)

Nmap determines the responsiveness of ports using Round-Trip Time (RTT). You can tune the time Nmap waits for responses to improve speed.

### Default Scan

```bash
sudo nmap 10.129.2.0/24 -F
```

Output:

```
Nmap done: 256 IP addresses (10 hosts up) scanned in 39.44 seconds
```

### Optimized RTT

```bash
sudo nmap 10.129.2.0/24 -F --initial-rtt-timeout 50ms --max-rtt-timeout 100ms
```

Output:

```
Nmap done: 256 IP addresses (8 hosts up) scanned in 12.29 seconds
```

| Option                  | Description                          |
| ----------------------- | ------------------------------------ |
| `--initial-rtt-timeout` | Sets initial wait time for response. |
| `--max-rtt-timeout`     | Sets maximum RTT wait time.          |

> ⚠️ Lower RTT values can speed up scans but may cause missed hosts.

---

## Retry Count

You can reduce the number of retries to improve scan speed. However, fewer retries increase the chance of missing open ports.

### Default Retries

```bash
sudo nmap 10.129.2.0/24 -F | grep "/tcp" | wc -l
# Output: 23
```

### Reduced Retries

```bash
sudo nmap 10.129.2.0/24 -F --max-retries 0 | grep "/tcp" | wc -l
# Output: 21
```

| Option          | Description                                         |
| --------------- | --------------------------------------------------- |
| `--max-retries` | Sets how many times to retry a port. Default is 10. |

> ⚠️ Setting retries to 0 skips any non-responsive ports immediately.

---

## Packet Rate

During white-box testing, we can safely increase scan rates to reduce total time.

### Default Rate

```bash
sudo nmap 10.129.2.0/24 -F -oN tnet.default
```

Output:

```
Scan time: 29.83 seconds
Open ports: 23
```

### Increased Rate

```bash
sudo nmap 10.129.2.0/24 -F -oN tnet.minrate300 --min-rate 300
```

Output:

```
Scan time: 8.67 seconds
Open ports: 23
```

| Option       | Description                 |
| ------------ | --------------------------- |
| `--min-rate` | Minimum packets per second. |

---

## Timing Templates (-T)

Nmap offers built-in timing templates that combine multiple performance settings.

| Template | Name       | Description                           |
| -------- | ---------- | ------------------------------------- |
| `-T0`    | Paranoid   | Extremely slow, evades IDS            |
| `-T1`    | Sneaky     | Very slow, evades IDS                 |
| `-T2`    | Polite     | Slower than default, reduces load     |
| `-T3`    | Normal     | Default template                      |
| `-T4`    | Aggressive | Speeds up scans, might trigger alerts |
| `-T5`    | Insane     | Fastest, very loud                    |

### Default Template

```bash
sudo nmap 10.129.2.0/24 -F -oN tnet.default
# Time: 32.44 seconds, Ports: 23
```

### Insane Template

```bash
sudo nmap 10.129.2.0/24 -F -oN tnet.T5 -T 5
# Time: 18.07 seconds, Ports: 23
```

> 🚀 Templates simplify optimization based on your scenario. Be careful—aggressive scans might get blocked!

---

## Resources

* [Performance and Timing](https://nmap.org/book/performance.html)
* [Timing Templates](https://nmap.org/book/performance-timing-templates.html)

---

By carefully adjusting these performance settings, you can significantly speed up your scans while balancing accuracy and stealth depending on your objective (pentesting vs. recon).
