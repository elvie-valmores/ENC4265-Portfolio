[Home](index.md) | [Manual Assessment Memo](manual_assessment_memo.md) | [Chatbot](chatbot.md) | [Procedure Video](procedure_video.md) | [Manual](manual.md) | [Reflective Blogs](reflective_blogs.md)

# Manual

# How to Use Wireshark for Network Traffic Analysis  
## *A Practical Guide for Beginners in Cybersecurity and Networking*

---

> ![Wireshark Interface Screenshot](https://github.com/user-attachments/assets/d571d09b-bf61-4d8b-8eb9-99b32b07f755)

> *Figure 1: Wireshark capturing live network traffic.*

---

> *This manual provides a hands-on, step-by-step approach to using Wireshark for analyzing network traffic. Whether you’re a cybersecurity student or a new analyst, this guide will teach you how to install Wireshark, capture packets, apply filters, and identify suspicious activity on a network.*

# Table of Contents

- [Introduction](#introduction)
  - [What You'll Learn](#what-youll-learn)
  - [Who This Manual Is For](#who-this-manual-is-for)
- [Installing and Setting Up Wireshark](#installing-and-setting-up-wireshark)
  - [System Requirements](#system-requirements)
  - [Installation Steps](#installation-steps)
  - [First-Time Configuration](#first-time-configuration)
- [Capturing Network Traffic](#capturing-network-traffic)
  - [Choosing the Right Network Interface](#choosing-the-right-network-interface)
  - [Starting a Capture](#starting-a-capture)
  - [Stopping and Saving a Capture](#stopping-and-saving-a-capture)
- [Analyzing Captured Packets](#analyzing-captured-packets)
  - [Understanding the Packet List Pane](#understanding-the-packet-list-pane)
  - [Packet Details Pane](#packet-details-pane)
  - [Follow TCP Stream](#follow-tcp-stream)
- [Using Display Filters](#using-display-filters)
  - [Basic Filters](#basic-filters)
  - [Common Protocol Filters](#common-protocol-filters)
  - [Combining Filters](#combining-filters)
- [Capture Filters vs. Display Filters](#capture-filters-vs-display-filters)
- [Network Protocols Deep Dive](#network-protocols-deep-dive)
  - [Understanding HTTP Traffic](#understanding-http-traffic)
  - [DNS and Name Resolution](#dns-and-name-resolution)
  - [TCP Three-Way Handshake](#tcp-three-way-handshake)
  - [ARP and MAC Resolution](#arp-and-mac-resolution)
- [Real-World Use Cases](#real-world-use-cases)
  - [Capturing a Login Session](#capturing-a-login-session)
  - [Detecting DNS Hijacking](#detecting-dns-hijacking)
  - [Spotting ARP Spoofing](#spotting-arp-spoofing)
- [Identifying Malicious or Suspicious Traffic](#identifying-malicious-or-suspicious-traffic)
  - [Unusual Protocols and Ports](#unusual-protocols-and-ports)
  - [DNS Tunneling and Suspicious Domains](#dns-tunneling-and-suspicious-domains)
  - [Examples of Attacks in Packet Captures](#examples-of-attacks-in-packet-captures)
- [Troubleshooting Network Issues with Wireshark](#troubleshooting-network-issues-with-wireshark)
- [Exporting and Sharing PCAP Files](#exporting-and-sharing-pcap-files)
- [Cleaning and Anonymizing PCAP Files](#cleaning-and-anonymizing-pcap-files)
- [Tips, Shortcuts, and Productivity Features](#tips-shortcuts-and-productivity-features)
- [Resources and Further Learning](#resources-and-further-learning)
- [Glossary of Terms](#glossary-of-terms)
- [AI Statement & References](#ai-statement--references)

---

# Introduction

Wireshark is one of the most powerful and widely used tools for network protocol analysis. It allows users to see exactly what’s happening on their network at a microscopic level.

This manual is intended to bridge the gap between theory and practice, giving you real-world steps to use Wireshark for security, troubleshooting, and educational purposes.

## What You'll Learn

- How to install and configure Wireshark  
- How to capture network traffic safely and effectively  
- How to apply filters and follow streams  
- How to identify indicators of compromise or suspicious behavior  
- How to troubleshoot connectivity issues using packet analysis

## Who This Manual Is For

- Cybersecurity students and IT learners  
- SOC analysts and interns starting with packet analysis  
- Networking professionals troubleshooting system issues  
- Anyone curious about what flows across their own home network

---

# Installing and Setting Up Wireshark

## System Requirements

- Windows 10/11, macOS, or Linux  
- At least 2 GB RAM  
- Admin/root access for packet capture permissions

## Installation Steps

1. Visit [https://www.wireshark.org/download.html](https://www.wireshark.org/download.html)  
2. Choose your OS and download the installer  
3. On Windows, check the box to install WinPcap/Npcap (required for packet capturing)  
4. Complete installation and restart if prompted

## First-Time Configuration

- Launch Wireshark  
- Set up capture permissions if prompted (Linux users may need to add to `wireshark` group)  
- Choose default interface and packet colorization options if desired

---

# Capturing Network Traffic

## Choosing the Right Network Interface

- Go to the **Capture Interfaces** window  
- Select your active internet-facing adapter (e.g., `Wi-Fi`, `Ethernet`)  
- Avoid `Loopback` unless testing local traffic

## Starting a Capture

1. Click the shark fin icon or double-click the interface name  
2. Let it run while you perform normal network activity  
3. Observe traffic populating in real-time

## Stopping and Saving a Capture

- Click the red square icon to stop  
- File → Save As → Name your `.pcapng` file for future analysis

---

# Analyzing Captured Packets

## Understanding the Packet List Pane

- Shows a summary of each packet captured  
- Includes timestamp, source, destination, protocol, and info

## Packet Details Pane

- Expands selected packet details by protocol layers  
- Click arrows to view Ethernet, IP, TCP, HTTP details

## Follow TCP Stream

- Right-click a TCP packet → **Follow** → **TCP Stream**  
- View entire conversation in readable format

---

# Using Display Filters

## Basic Filters

- `ip.addr == 192.168.0.1` — traffic to/from a specific IP  
- `tcp.port == 80` — traffic over a specific port  
- `http` — only HTTP traffic

## Common Protocol Filters

- `dns`, `ftp`, `smtp`, `tls`, `icmp`, `arp`  
- Combine with `and`, `or`, `not`

## Combining Filters

- `ip.addr == 10.0.0.2 and tcp.port == 443`  
- `http.request.method == "GET"`

---

# Capture Filters vs. Display Filters

**Capture filters** are applied before data is collected and help limit what Wireshark records.  
**Display filters** are used after the capture to narrow down what you view.

### Examples of Capture Filters

- `host 192.168.0.1` — only capture traffic to/from one IP address  
- `port 443` — only capture HTTPS traffic  
- `net 10.0.0.0/8` — only capture traffic within a local subnet  

> *Use capture filters to reduce data size and performance load on large networks.*

---

# Network Protocols Deep Dive

## Understanding HTTP Traffic

- Look at `GET` and `POST` requests  
- Check for credentials or API keys in the payload  
- HTTP headers reveal user agents, cookies, and session tokens

## DNS and Name Resolution

- DNS queries (`Standard query`) and responses (`Standard query response`)  
- Track lookups for suspicious domains or excessive requests  
- Filter: `dns.qry.name contains "suspicious.com"`

## TCP Three-Way Handshake

- Syn → Syn+Ack → Ack is the normal connection setup  
- Packet loss or retransmission may indicate connectivity issues  
- Filter: `tcp.flags.syn == 1 and tcp.flags.ack == 0`

## ARP and MAC Resolution

- Watch for gratuitous ARP replies or ARP flooding  
- Signs of spoofing: one MAC address claiming many IPs

---

# Real-World Use Cases

## Capturing a Login Session

- Filter for `http` or `ftp` traffic  
- Follow TCP stream to reveal credentials (in insecure protocols)

## Detecting DNS Hijacking

- Look for DNS responses with unexpected IPs  
- Repeated lookups for the same domain with different answers

## Spotting ARP Spoofing

- Multiple ARP replies from the same MAC for different IPs  
- Use `arp` filter and enable MAC column in the packet list

---

# Identifying Malicious or Suspicious Traffic

## Unusual Protocols and Ports

- Look for unexpected traffic on ports like 6667 (IRC), 23 (Telnet)  
- Filter: `tcp.port > 1024 and tcp.flags.syn == 1`

## DNS Tunneling and Suspicious Domains

- Look for large DNS packets or frequent requests to unknown domains  
- Filter: `dns.qry.name contains ".xyz"`

## Examples of Attacks in Packet Captures

- **ARP Spoofing**: Multiple ARP replies from same MAC  
- **Credential Leak**: Cleartext usernames in `http` or `ftp`

---

# Troubleshooting Network Issues with Wireshark

- **Slow Connections**: Use `tcp.analysis.flags` to find retransmissions  
- **DHCP Issues**: Look for `bootp` traffic and lease offers  
- **SSL/TLS Errors**: Look at `tls.handshake` details or failed certs

---

# Exporting and Sharing PCAP Files

- Use **File → Export Specified Packets** for filtered results  
- Anonymize data if needed using **Editcap** or **TraceWrangler**  
- Share `.pcap` with security teams for collaborative analysis

---

# Cleaning and Anonymizing PCAP Files

Before sharing `.pcap` files with classmates or colleagues, consider cleaning sensitive info.

- Use **Editcap** to remove packets  
- Use **TraceWrangler** to anonymize IP/MAC addresses  
- Never share login credentials or real user data in unfiltered captures

---

# Tips, Shortcuts, and Productivity Features

- Press `Ctrl + /` to open the filter cheat sheet  
- Right-click any packet field → **Apply as Filter** → **Selected**  
- Use **Coloring Rules** to highlight suspicious protocols  
- Enable **Time Display Format** as "Seconds Since Beginning of Capture" for timeline analysis

---

# Resources and Further Learning

- [Wireshark User Guide](https://www.wireshark.org/docs/wsug_html_chunked/) — Official documentation for full feature use  
- [Wireshark Q&A Site](https://ask.wireshark.org/) — Community-driven troubleshooting and advice  
- [CloudShark](https://www.cloudshark.org/) — Analyze PCAPs in the cloud  
- [Malware Traffic Analysis](https://www.malware-traffic-analysis.net/) — Practice PCAPs with write-ups  
- [Security Onion](https://securityonion.net) — Network monitoring platform using Wireshark, Suricata, and others

---

# Glossary of Terms

- **Packet** — A single unit of data sent over a network  
- **PCAP** — File format used to save captured packet data  
- **Display Filter** — A syntax rule in Wireshark to view specific packets  
- **Capture Filter** — A rule applied during packet capture to limit traffic  
- **Three-Way Handshake** — TCP connection setup using SYN, SYN-ACK, ACK  
- **Stream** — A sequence of packets belonging to a single session (e.g., TCP stream)  
- **ARP Spoofing** — A technique where an attacker sends fake ARP messages to associate their MAC address with another IP  
- **DNS Tunneling** — Using DNS queries to exfiltrate data

---

# AI Statement & References

## AI Tools and Software Used

- **ChatGPT-4o** – For outlining and organizing the manual content  
- **Grammarly** – For clarity, spelling, and readability checking

## References

- Wireshark Documentation – [https://www.wireshark.org](https://www.wireshark.org)  
- Malware-Traffic-Analysis.net – Sample PCAPs and incident reviews  
- SANS Institute — Capture the Flag (CTF) training examples  
- CloudShark — Online PCAP visualizations and training  
- Official Nmap and Metasploit documentation for packet behavior context

---

> *Thank you for reading this manual! Use Wireshark responsibly and always ensure proper permissions when capturing network traffic.*
