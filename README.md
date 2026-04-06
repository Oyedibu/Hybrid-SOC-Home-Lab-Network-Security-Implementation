# Hybrid-SOC-Home-Lab-Network-Security-Implementation
This project involved the architectural design and deployment of a multi-segemented virtual network environment using pfSense and Hyper V. The core objective was to establish a secure "Testing" zone for malware and phishing analysis that is logically isolated from the management network to prevent lateral movement.
# Network Architecture
    The environment is divided into three distinct security zones:
    . WAN Interface: Connects to the physical internet gateway
    . LAN(Management) Segment: Dedicated to the KALI Linux attack and monitoring machine (10.0.0.102)
    . Testing(Isolated) Segment: A dedicated VLAN for victim machines and malware execution (10.0.30.100)
<img width="1366" height="768" alt="Screenshot (29)" src="https://github.com/user-attachments/assets/84bc7959-f39c-4cfd-8e59-4f6d5638275c" />
# Security Policy & Implementation
    Using pfSense, I implemented a Zero-Trust egress policy for the Testing segment.
    .Rule 1: Explicitly allow DNS and DHCP for client connectivity.
    .Rule 2: Block all traffic from the Testing segment to the Management LAN
    .Rule 3: Allow Restricted internet access for threat research
<img width="1366" height="768" alt="Screenshot (31)" src="https://github.com/user-attachments/assets/04108048-a3da-4cbd-82f4-64307fcb1c14" />
# Technical Challenges & Troubleshooting
    During the deployment , a routing conflict occured where the management  machine(Kali) retained legacy IP configuration leading to a       "Destination Host Unreachable" error.
    . Issue: Conflict between the 192.168.100.10 legacy static IP and the new 10.0.0.102 DHCP lease
    . Resolution: Performed a network interface flush and renewed the DHCP lease to restore WAN connectivity

# Forensic Validation (Wireshark Analysis)
    To verify the integrity of the security boundary, I conducted a deep packet inspection on the traffic crossing gateway.
    . Validation: Wireshark captures successfully identified ICMP Destination Unreachable packets when the Victim VM attempted to ping the       Management gateway.
    . Result: This confirm that the firewall is sucessfully dropping unauthorized lateral movement attempts.
<img width="1366" height="768" alt="Screenshot (32)" src="https://github.com/user-attachments/assets/8f38424f-7baa-4a1d-ae9c-c649526a17ee" />
# Email Security and Phishing Investigation
    I used the lab also to serve as a sandbox for email header forensics.
    . Investigation: Analyzed a spoofed "IT support" email using MXToolbox
    . Findings: Identified as SPF Failure, indicating the source IP was unauthorized to send on behalf of the domain.
<img width="1080" height="2400" alt="Screenshot_20260405-193732" src="https://github.com/user-attachments/assets/a73d2220-73e7-4afa-96ec-14eae7e06ef5" />
# Skills Demonstrated
    . Network Micro-segmentation
    . Firewall Rule Configuration(pfSense)
    . Packet Analysis & Forensic Validation (Wireshark)
    . Virtual infrastructure management (Hyper V)
# Tools Used
    . Hypervisor: Hyper-V
    . Firewall/Router: pfSense
    . Operating System: Kali Linux, Windows 10
    . Analysis Tools: Wireshark
