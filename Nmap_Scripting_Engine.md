# Nmap Scripting Engine

The **Nmap Scripting Engine (NSE)** is a powerful feature of Nmap that allows users to write and use scripts to automate tasks like service discovery, vulnerability detection, and brute-force attacks. Scripts are written in **Lua** and stored in various categories, allowing targeted and flexible scans.

---

## Script Categories

| Category    | Description                                                            |
| ----------- | ---------------------------------------------------------------------- |
| `auth`      | Determines authentication credentials.                                 |
| `broadcast` | Discovers hosts via broadcast and can add them to scans automatically. |
| `brute`     | Performs brute-force attacks using known credential lists.             |
| `default`   | The default scripts run with `-sC`.                                    |
| `discovery` | Evaluates accessible services for further enumeration.                 |
| `dos`       | Tests for Denial of Service vulnerabilities (less used).               |
| `exploit`   | Tries to exploit known vulnerabilities on scanned services.            |
| `external`  | Uses external services for additional processing.                      |
| `fuzzer`    | Sends varied packets to identify input-handling issues.                |
| `intrusive` | May affect the target system; includes aggressive tests.               |
| `malware`   | Detects signs of malware or malicious behavior.                        |
| `safe`      | Non-intrusive scripts safe for general use.                            |
| `version`   | Enhances service version detection.                                    |
| `vuln`      | Identifies known vulnerabilities based on service fingerprints.        |

---

## Running NSE Scripts

### 1. Default Scripts

Run a scan with the default script set:

```bash
sudo nmap <target> -sC
```

### 2. Category-Specific Scripts

Run all scripts in a category:

```bash
sudo nmap <target> --script <category>
```

Example:

```bash
sudo nmap 10.129.2.28 --script vuln
```

### 3. Specific Scripts

Specify one or more individual scripts:

```bash
sudo nmap <target> --script banner,smtp-commands
```

Sample Output:

```bash
PORT   STATE SERVICE
25/tcp open  smtp
|_banner: 220 inlane ESMTP Postfix (Ubuntu)
|_smtp-commands: inlane, PIPELINING, SIZE 10240000, VRFY, ETRN, STARTTLS, ENHANCEDSTATUSCODES, 8BITMIME, DSN, SMTPUTF8,
```

These results show:

* The server runs **Postfix** on **Ubuntu**.
* Supported SMTP commands can assist in further enumeration.

---

## Aggressive Scan Option (-A)

This option combines multiple advanced scan features:

```bash
sudo nmap 10.129.2.28 -p 80 -A
```

Includes:

* Service Version Detection (`-sV`)
* OS Detection (`-O`)
* Traceroute (`--traceroute`)
* Default NSE scripts (`-sC`)

Sample Output:

```bash
PORT   STATE SERVICE VERSION
80/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))
|_http-title: blog.inlanefreight.com
|_http-server-header: Apache/2.4.29 (Ubuntu)
|_http-generator: WordPress 5.3.4
```

This gives insight into:

* The web application and version
* The technology stack
* OS fingerprint

---

## Vulnerability Scanning

Use the `vuln` category to check for known vulnerabilities:

```bash
sudo nmap 10.129.2.28 -p 80 -sV --script vuln
```

Sample Results:

```bash
| http-enum:
|   /wp-login.php: Possible admin folder
|   /: WordPress version: 5.3.4
| http-wordpress-users:
|   Username found: admin
| vulners:
|   CVE-2019-0211  https://vulners.com/cve/CVE-2019-0211
|   CVE-2018-1312  https://vulners.com/cve/CVE-2018-1312
```

---

## Summary of Scan Options

| Option         | Description                                       |
| -------------- | ------------------------------------------------- |
| `-sC`          | Run default scripts                               |
| `--script <x>` | Run specified script(s) or categories             |
| `-A`           | Aggressive scan: combines several useful features |
| `-sV`          | Enable service version detection                  |

---

## Learn More

For a full list of scripts and usage, check:
🔗 [https://nmap.org/nsedoc/](https://nmap.org/nsedoc/)

Explore and combine these scripts to automate information gathering, enhance reconnaissance, and uncover vulnerabilities efficiently!

---
Questions

Answer the question(s) below to complete this Section!
Spawn the Target in the module specific Lab.

Q) Use NSE and its scripts to find the flag that one of the services contain and submit it as the answer.
`Answer: HTB{873nniuc71bu6usbs1i96as6dsv26}`

---
