# Wireshark Network Traffic Analysis

Practical packet-analysis exercises covering common attack patterns and a separate suspicious HTTP traffic investigation. Each exercise includes a written finding, a PCAPNG capture, and supporting screenshots where available.

> All simulations were performed in an isolated, authorized lab. The findings describe observed traffic and do not by themselves prove compromise.

## At A Glance

| Area | Coverage |
| --- | --- |
| Protocols | TCP, UDP, ICMP, ARP, SSH, HTTP, and DNS |
| Detection themes | Scanning, floods, spoofing, repeated authentication failures, and suspicious web requests |
| Analysis tool | Wireshark |
| Lab systems | Kali Linux and Windows 10 running in VMware |

## Investigations

| # | Investigation | Report | Capture |
| --- | --- | --- | --- |
| 01 | TCP SYN port scan | [Read the analysis](01-TCP-SYN-Port-Scan/analysis.md) | [Open the PCAPNG](pcaps/01_tcp_syn_port_scan.pcapng) |
| 02 | ICMP flood | [Read the analysis](02-ICMP-Flood/analysis.md) | [Open the PCAPNG](pcaps/02_icmp_flood.pcapng) |
| 03 | TCP SYN flood | [Read the analysis](03-TCP-SYN-Flood/analysis.md) | [Open the PCAPNG](pcaps/03_tcp_syn_flood.pcapng) |
| 04 | ARP spoofing attempt | [Read the analysis](04-ARP-Spoofing/analysis.md) | [Open the PCAPNG](pcaps/04_arp_spoof.pcapng) |
| 05 | UDP flood | [Read the analysis](05-UDP-Flood/analysis.md) | [Open the PCAPNG](pcaps/05_udp_flood.pcapng) |
| 06 | SSH brute-force simulation | [Read the analysis](06-SSH-Brute-Force/analysis.md) | [Open the PCAPNG](pcaps/06_ssh_bruteforce.pcapng) |

## Lab Environment

| Component | Details |
| --- | --- |
| Attacker / test system | Kali Linux, `192.168.245.132` |
| Target system | Windows 10, `192.168.245.141` |
| Gateway used for ARP exercise | `192.168.245.2` |
| Virtualization | VMware, NAT network |
| Packet analysis | Wireshark |
| Traffic-generation tools | Nmap, `ping`, Nping, `arpspoof`, and Hydra |

## Repository Layout

```text
.
├── 01-TCP-SYN-Port-Scan/analysis.md
├── 02-ICMP-Flood/analysis.md
├── 03-TCP-SYN-Flood/analysis.md
├── 04-ARP-Spoofing/analysis.md
├── 05-UDP-Flood/analysis.md
├── 06-SSH-Brute-Force/analysis.md
├── pcaps/                         # Packet captures for the six exercises
├── evidence/                      # Wireshark and host screenshots
└── README.md
```

## Supplemental Suspicious-Traffic Investigation

The evidence set also documents a separate traffic-analysis exercise involving potentially suspicious HTTP activity.

| Field | Observation |
| --- | --- |
| Internal host | `10.2.28.8` |
| Remote IP | `45.131.214.85` |
| Request | `POST /fakeurl.htm` |
| User-Agent | `NetSupport Manager/1.3` |
| Response | `200 OK` |
| Form field | `CMD` |

Repeated POST requests, command-like form data, and the User-Agent are investigation leads. They are not conclusive proof of malicious activity. Endpoint telemetry, process data, DNS history, and threat-intelligence checks would be needed for validation.

## Useful Wireshark Filters

| Filter | Use |
| --- | --- |
| `icmp.type == 8` | ICMP Echo Requests |
| `ip.addr == 192.168.245.141` | Traffic involving the Windows host |
| `tcp.port == 22` | SSH traffic |
| `ip.addr == 45.131.214.85` | Traffic involving the suspicious remote IP |
| `http.request.method == "POST"` | HTTP POST requests |
| `http.response` | HTTP responses |
| `frame.number == 2639` | A specific packet by frame number |

## Suggested Analysis Workflow

1. Open a capture from `pcaps/` in Wireshark.
2. Review the protocol hierarchy, endpoints, conversations, and packet counts.
3. Identify unusual addresses, ports, rates, and protocol behavior.
4. Apply focused display filters and inspect packet details.
5. Follow the relevant TCP stream when useful.
6. Save supporting screenshots in `evidence/`.
7. Separate direct observations from interpretation and record follow-up questions.

## Key Skills Practiced

- Recognizing scanning, flooding, spoofing, and repeated authentication patterns.
- Using Wireshark filters to narrow large captures.
- Correlating packet observations with Windows Security Event ID `4625`.
- Investigating HTTP requests, response codes, and encoded form data.
- Writing evidence-based conclusions in a SOC L1 investigation style.

## Disclaimer

This project is for education and authorized defensive testing only. Do not scan, flood, spoof, or test authentication against systems or networks without explicit permission.

## Author

**Parth Joshi**  
Cybersecurity learner     
[GitHub: parth6213](https://github.com/parth6213)
