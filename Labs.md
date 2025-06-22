Disclaimer: Perform this in HTB Lab of Network Enumeration with Nmap

# Firewall and IDS/IPS Evasion - Easy Lab

Now let's get practical. A company hired us to test their IT security defenses, including their IDS and IPS systems. Our client wants to increase their IT security and will, therefore, make specific improvements to their IDS/IPS systems after each successful test. We do not know, however, according to which guidelines these changes will be made. Our goal is to find out specific information from the given situations.

We are only ever provided with a machine protected by IDS/IPS systems and can be tested. For learning purposes and to get a feel for how IDS/IPS can behave, we have access to a status web page at:

```
http://<target>/status.php
```

This page shows us the number of alerts. We know that if we receive a specific amount of alerts, we will be banned. Therefore we have to test the target system as quietly as possible.

### Questions

* **1**  Our client wants to know if we can identify which operating system their provided machine is running on. Submit the OS name as the answer.
  **Answer:** Ubuntu

---

# Firewall and IDS/IPS Evasion - Medium Lab

After we conducted the first test and submitted our results to our client, the administrators made some changes and improvements to the IDS/IPS and firewall. We could hear that the administrators were not satisfied with their previous configurations during the meeting, and they could see that the network traffic could be filtered more strictly.

> **Note:** To successfully solve the exercise, we must use the UDP protocol on the VPN.

### Questions

* **1**  After the configurations are transferred to the system, our client wants to know if it is possible to find out our target's DNS server version. Submit the DNS server version of the target as the answer.
  **Answer:** HTB{GoTtgUnyze9Psw4vGjcuMpHRp}

---

# Firewall and IDS/IPS Evasion - Hard Lab

With our second test's help, our client was able to gain new insights and sent one of its administrators to a training course for IDS/IPS systems. As our client told us, the training would last one week. Now the administrator has taken all the necessary precautions and wants us to test this again because specific services must be changed, and the communication for the provided software had to be modified.

### Questions

* **2**  Now our client wants to know if it is possible to find out the version of the running services. Identify the version of service our client was talking about and submit the flag as the answer.
  **Answer:** HTB{kjnsdf2n982n1827eh76238s98di1w6}

---
