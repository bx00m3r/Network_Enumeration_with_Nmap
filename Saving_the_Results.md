# Saving the Results

### Different Formats

While we run various scans, we should always save the results. We can use these later to examine the differences between the different scanning methods we have used. Nmap can save the results in 3 different formats:

* **Normal output** (`-oN`) with the `.nmap` file extension
* **Grepable output** (`-oG`) with the `.gnmap` file extension
* **XML output** (`-oX`) with the `.xml` file extension

We can also specify the option `-oA` to save the results in **all formats**. The command could look like this:

```bash
sudo nmap 10.129.2.28 -p- -oA target
```

Sample Output:

```bash
Starting Nmap 7.80 ( https://nmap.org ) at 2020-06-16 12:14 CEST
Nmap scan report for 10.129.2.28
Host is up (0.0091s latency).
Not shown: 65525 closed ports
PORT      STATE SERVICE
22/tcp    open  ssh
25/tcp    open  smtp
80/tcp    open  http
MAC Address: DE:AD:00:00:BE:EF (Intel Corporate)
```

### Scanning Options

| Option       | Description                                                                    |
| ------------ | ------------------------------------------------------------------------------ |
| 10.129.2.28  | Scans the specified target.                                                    |
| `-p-`        | Scans **all** ports.                                                           |
| `-oA target` | Saves results in all formats (`.nmap`, `.gnmap`, `.xml`) with prefix `target`. |

> If no full path is given, the results will be stored in the directory we are currently in.

### File Outputs

```bash
ls
# Output:
target.gnmap target.xml  target.nmap
```

---

### Normal Output (`.nmap`)

```bash
cat target.nmap
```

```
PORT   STATE SERVICE
22/tcp open  ssh
25/tcp open  smtp
80/tcp open  http
```

---

### Grepable Output (`.gnmap`)

```bash
cat target.gnmap
```

```
Host: 10.129.2.28 () Ports: 22/open/tcp//ssh///, 25/open/tcp//smtp///, 80/open/tcp//http///
```

---

### XML Output (`.xml`)

```bash
cat target.xml
```

```
<port protocol="tcp" portid="22"><state state="open" ... /><service name="ssh" /></port>
<port protocol="tcp" portid="25"><state state="open" ... /><service name="smtp" /></port>
<port protocol="tcp" portid="80"><state state="open" ... /><service name="http" /></port>
```

---

### Style Sheets and HTML Report

With the XML output, we can easily create HTML reports that are easy to read, even for non-technical people. This is very useful for documentation.

Convert XML to HTML:

```bash
xsltproc target.xml -o target.html
```

Open the `target.html` file in your browser for a neat, structured result.

---

### Summary Nmap Report:

```
Nmap scan report for IP 10.10.10.28 shows open ports: 22 (SSH), 25 (SMTP), 80 (HTTP). Scanned on June 16, 2020.
```

---

For more on output formats, see: [https://nmap.org/book/output.html](https://nmap.org/book/output.html)

---

Questions

Perform on the target machine provided in HTB Academy module specific Lab or even on your own Lab/System!
Answer the question(s) below to complete this Section!

Perform a full TCP port scan on your target and create an HTML report. Submit the number of the highest port as the answer.
`Answer: 31337`

---
