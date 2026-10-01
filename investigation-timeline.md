# Investigation Timeline

## Phase 1 — Network Configuration
- Windows identified as 192.168.100.8/24.
- Kali identified as 192.168.100.108/24 on eth0.
- Router/default gateway identified as 192.168.100.1.

## Phase 2 — Connectivity
- Initial ping test failed.
- Corrected testing achieved 0% packet loss.
- Bidirectional host communication confirmed.

## Phase 3 — Host Discovery
- `sudo nmap -sn 192.168.100.0/24`
- 13 active hosts identified.
- Huawei, Intel, Nintendo and Espressif vendor associations observed.

## Phase 4 — Service Enumeration
- Router service baseline collected.
- Windows TCP/7070 identified.
- Kali showed no responsive TCP services in the 1,000-port scan.

## Phase 5 — Live Capture
- Wireshark capture saved as `live.pcapng`.
- 2,592 packets observed.
- 0 dropped packets reported.

## Phase 6 — DNS
- Normal Mozilla/Google DNS activity observed.
- Repeated `trycloudflare.com` A/AAAA queries returned NXDOMAIN.
- Activity classified as requiring correlation, not confirmed malicious.

## Phase 7 — TLS
- TLS/HTTPS traffic inspected.
- `api.ipify.org` observed in a separate TLS handshake.
- No direct correlation to the failed trycloudflare lookups established.

## Phase 8 — ARP
- Normal local IP-to-MAC resolution observed.
- No duplicate IP/MAC ownership demonstrated.

## Phase 9 — TCP
- Multiple TCP/443 encrypted conversations observed.
- Router HTTP communication also observed.
- Conversation statistics used to identify major traffic relationships.

## Phase 10 — IPv4 Endpoints
- 20 IPv4 endpoints observed in the unfiltered endpoint view.
- Kali generated approximately 2 MB of observed traffic.
- `57.144.38.128` was the largest endpoint by packet count.

## Final Assessment
No definitive malicious activity was identified in the captured traffic. Follow-up areas include unknown devices, Windows TCP/7070, recurring DNS anomalies, and improving LAN-wide sensor visibility.
