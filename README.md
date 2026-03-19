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

###ARP Reply Analysis
```bash
arp.opcode == 2
```
ARP replies were analysed, as these are commonly used in spoofing attacks where attackers send forged responses.

###Gateway Investigation
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
ARP Traffic Overview
![image](https://github.com/sjmercene/sjmercene/blob/a6a60f2962be7ea5fec76426c4065155db065e6f/ARP%20Traffic%20Overview.jpg)
This screenshot shows ARP traffic after applying the arp filter, establishing a baseline of network communication.

ARP Replies (Attack Focus)
![image](https://github.com/sjmercene/sjmercene/blob/f1031afdf96173b08110ca2f28695f8310081222/ARP%20Replies.png)
This view highlights ARP reply packets, which are commonly used by attackers to perform spoofing.

Gateway Investigation
![image](https://github.com/sjmercene/sjmercene/blob/f1031afdf96173b08110ca2f28695f8310081222/Gateway%20Investigation.png)
Multiple MAC addresses are shown for the same IP address, confirming suspicious activity.
