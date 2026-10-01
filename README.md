# Home Network Live Traffic Analysis & Security Monitoring

## Overview

This project documents an authorized live network-security assessment of a home LAN using Kali Linux, Windows, Nmap, and Wireshark.

The objective was to establish a network baseline, identify active hosts and services, capture live traffic, investigate DNS/ARP/TCP/TLS/QUIC behavior, and document security observations using packet-level evidence.

> **Scope:** Only the user's own home network and authorized systems were assessed.

## Lab Architecture

```text
                    INTERNET
                       |
                +--------------+
                | Home Router  |
                | 192.168.100.1|
                +------+-------+
                       |
               192.168.100.0/24
                  +----+----+
                  |         |
            Windows       Kali VM
        192.168.100.8  192.168.100.108
                         eth0 / Wireshark
```

## Tools

- Kali Linux
- Windows
- VirtualBox
- Nmap
- Wireshark

## Methodology

1. Configure Kali and Windows on the same LAN.
2. Verify bidirectional connectivity.
3. Discover active hosts with `nmap -sn`.
4. Enumerate services on selected known systems with `nmap -sV`.
5. Capture live traffic with Wireshark.
6. Analyze DNS traffic and investigate repeated NXDOMAIN queries.
7. Inspect TLS/SNI information.
8. Analyze ARP behavior.
9. Analyze TCP conversations.
10. Review unfiltered IPv4 endpoints.
11. Produce evidence-based findings and recommendations.

## Key Results

### Network

- Windows: `192.168.100.8/24`
- Kali: `192.168.100.108/24`
- Router: `192.168.100.1`
- Network: `192.168.100.0/24`
- Nmap host discovery: **13 active hosts**

### Service Baseline

Router `192.168.100.1`:
- `53/tcp` open — DNS
- `80/tcp` open — HTTP
- `1900/tcp` reported tcpwrapped
- `21/tcp`, `22/tcp`, `23/tcp`, `8022/tcp` filtered

Windows `192.168.100.8`:
- `7070/tcp` reported open as `ssl/realserver?`; exact service not confirmed

Kali `192.168.100.108`:
- No responsive service among the 1,000 TCP ports scanned

## Wireshark Findings

The baseline capture contained **2,592 packets** with **0 dropped packets**.

Observed traffic included:

- DNS
- ARP
- TCP/443
- TLS 1.2/1.3
- QUIC/UDP 443
- Router HTTP communication
- Mozilla/Firefox-related traffic
- Multiple external IPv4 endpoints

### DNS Investigation

Repeated DNS requests were observed for:

- `michigan-glory-flexibility-skill.trycloudflare.com`
- `cleaner-gods-complete-reactions.trycloudflare.com`

The observed responses were NXDOMAIN.

These names were recorded as **requiring investigation**, not as confirmed malicious indicators.

### TLS Investigation

A separate TLS connection to `104.26.13.205:443` exposed `api.ipify.org` in handshake data. This did not establish a direct relationship with the failed `trycloudflare.com` lookups.

### ARP

Normal IP-to-MAC resolution was observed. The capture did not demonstrate duplicate ownership of the same IP by different MAC addresses.

### Endpoint Analysis

The unfiltered IPv4 endpoint view contained **20 endpoints**. Kali generated approximately **2 MB** of observed traffic. `57.144.38.128` was the largest external endpoint by packet count in the capture.

## Security Assessment

No definitive evidence of malicious activity was identified in the captured traffic.

Important follow-up items:

1. Identify remaining unknown LAN devices.
2. Validate the application/service on Windows TCP/7070.
3. Investigate the source application responsible for repeated `trycloudflare.com` DNS lookups if the behavior recurs.
4. Review router management exposure.
5. Improve monitoring architecture if full LAN-wide visibility is required.

## Limitations

- The capture was short and represents a baseline, not continuous monitoring.
- Kali was the primary capture point.
- Bridged Wi-Fi virtualization does not automatically provide complete visibility into every wireless client's unicast traffic.
- Encrypted traffic limits application-payload visibility.
- MAC-vendor identification does not prove device identity.
- DNS anomalies alone do not prove compromise.

## Evidence

The `evidence/` directory should contain the screenshots referenced in the investigation report.

## Project Structure

```text
home-network-live-analysis/
├── README.md
├── investigation-report.pdf
├── investigation-report.docx
├── captures/
│   └── live.pcapng
├── evidence/
│   ├── 01_windows_ip_configuration.png
│   ├── 02_kali_ip_configuration.png
│   ├── 03_connectivity_test.png
│   ├── 05_network_discovery_nmap.png
│   ├── 06_router_service_scan.png
│   ├── 07_windows_service_scan.png
│   ├── 08_kali_service_scan.png
│   ├── 09_live_traffic_capture.png
│   ├── 10_dns_traffic_analysis.png
│   ├── 11_trycloudflare_dns_activity.png
│   ├── 12_tls_sni_correlation.png
│   ├── 13_arp_analysis.png
│   ├── 14_tcp_conversations.png
│   └── 16_ipv4_endpoints.png
└── findings/
    └── investigation-timeline.md
```

## Skills Demonstrated

- Network discovery
- Nmap host/service enumeration
- Wireshark packet analysis
- DNS investigation
- ARP analysis
- TCP conversation analysis
- TLS/SNI inspection
- Network baseline development
- Evidence handling
- Security finding classification
- SOC-style investigation documentation
