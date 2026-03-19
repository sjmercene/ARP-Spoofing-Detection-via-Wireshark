# 📡 ARP Spoofing Detection Lab (Wireshark)

## 📌 Overview
This project documents a network traffic analysis investigation to detect an ARP spoofing (Man-in-the-Middle) attack.  
The objective was to analyse captured packets using Wireshark and identify abnormal ARP behaviour indicating malicious activity.

---

## 🎯 Objectives
- Analyse ARP traffic using Wireshark  
- Identify abnormal ARP behaviour  
- Detect signs of ARP spoofing (MITM attack)  
- Investigate suspicious IP-to-MAC mappings  
- Understand how attackers manipulate network traffic  

---

##  Environment
- **Tool:** Wireshark  
- **Platform:** Kali Linux  
- **Lab Environment:** TryHackMe  

---

##  Investigation

### ARP Traffic Filtering
```bash
arp
```
ARP traffic was isolated to focus only on address resolution activity within the network.

### ARP Reply Analysis
```bash
arp.opcode == 2
```
ARP replies were analysed, as these are commonly used in spoofing attacks where attackers send forged responses.

### Gateway Investigation
```bash
arp.opcode == 2 && arp.src.proto_ipv4 == 192.168.10.1
```
Filtering for the gateway IP revealed multiple MAC addresses associated with the same IP.

### Findings

- Two different MAC addresses were observed claiming the same IP address:

- Legitimate Gateway: 02:aa:bb:cc:00:01
- Suspicious (Attacker): 02:fe:fe:fe:55:55
- Indicators of ARP Spoofing
- Duplicate IP-to-MAC mappings detected
- Repeated ARP replies for the same IP
- Presence of gratuitous ARP responses
- High frequency of ARP traffic

### Evidence
Wireshark packet analysis showed repeated responses such as:
"192.168.10.1 is at 02:fe:fe:fe:55:55"
These responses were unsolicited and frequent, indicating spoofing behaviour.

## 📸 Screenshots
## ARP Traffic Overview
![image](https://github.com/sjmercene/sjmercene/blob/a6a60f2962be7ea5fec76426c4065155db065e6f/ARP%20Traffic%20Overview.jpg)
This screenshot shows ARP traffic after applying the arp filter, establishing a baseline of network communication.

## ARP Replies (Attack Focus)
![image](https://github.com/sjmercene/sjmercene/blob/f1031afdf96173b08110ca2f28695f8310081222/ARP%20Replies.png)
ARP replies were analysed because they are commonly exploited in ARP spoofing attacks.
In normal network behaviour:
A device responds to an ARP request with its legitimate MAC address
Each IP address should map to a single, consistent MAC address
However, attackers can send forged ARP replies without being requested (gratuitous ARP), claiming:
"192.168.10.1 is at attacker MAC"

## Gateway Investigation
![image](https://github.com/sjmercene/sjmercene/blob/f1031afdf96173b08110ca2f28695f8310081222/Gateway%20Investigation.png)
<br>The gateway (192.168.10.1) is a critical network device that routes traffic between the local network and external networks (e.g. the internet).
Because all outbound traffic passes through the gateway, it is a prime target for attackers performing Man-in-the-Middle (MITM) attacks.
During analysis, multiple MAC addresses were observed claiming to be the gateway:
- Legitimate MAC address
- Suspicious MAC address (attacker)
This is abnormal, as a single IP address should only map to one MAC address.
The presence of multiple MAC addresses for the gateway IP strongly indicates ARP spoofing, where the attacker is impersonating the gateway to intercept network traffic.
