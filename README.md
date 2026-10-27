# NetSentinel Pro – Network Monitoring Tool (Windows x64)

Single-file, self-contained Windows x64 app (no .NET install needed): `NetSentinel.exe`.

## Features

| Area | What it does |
|---|---|
| **Discovery scan** | Finds every device on a range (`192.168.1.0/24`, `10.0.0.1-254`, lists). Uses ICMP, ARP and TCP probes, so hosts that block ping are still found. Collects MAC, vendor, DNS and NetBIOS names, workgroup, logged-on user, open ports, service banners, web page titles, SNMP sysDescr/location and UPnP model/serial. Classifies each device by type and OS (PC, server, router, switch, printer, camera, NAS, AP, VoIP, IoT, VM…). |
| **Switch discovery** | **Network Tools → Switch Discovery** finds every kind of switch and lists **IP, MAC, vendor, model, serial number**, firmware, port count and where each one is connected. **Managed:** SNMP bridge MIB, ENTITY-MIB chassis model/serial (vendor serial OIDs as fallback), LLDP/CDP neighbours (followed to switches outside the range) and the MAC address table (Cisco per-VLAN supported). **Smart / web-managed:** web UI fingerprinting (NETGEAR, TP-Link, D-Link, Cisco SB, Aruba, Ubiquiti, Zyxel, MikroTik and more), NETGEAR NSDP and MikroTik MNDP broadcasts. **Unmanaged:** these have no IP, so they are inferred from managed switches' MAC tables (several hosts on one access port) and listed with the hosts behind them. Results can be exported or added to Devices. |
| **Switch path** | **Network Tools → Switch Path** (or right-click a device → *Trace switch path*) lists every switch between this server and a destination, in order, with model, serial, IP, MAC and the ports facing each end. It reads fresh MAC tables from the managed switches (a switch is on the path when it has learned the server and the destination on different ports). Routed destinations are traced as server → gateway, the routed hops (traceroute), then last router → destination. Unmanaged switches in the path are flagged from MAC tables. Runs Switch Discovery automatically if no SNMP switches are known yet. |
| **Hardware & software inventory** | **Windows (WMI):** system, BIOS, serial, CPU, RAM modules, disks, volumes, GPU, monitors, NICs, installed software, updates, services, processes, startup items, shares, printers, local users and admins, antivirus. **Network gear, printers, Linux, NAS (SNMP):** interfaces, IPs, storage, software, processes, toner levels, LLDP neighbours, ARP table. |
| **Filters & search** | Global **Search** button (top bar) plus a per-page search. Filters for Status, Type, Vendor, OS, Location, Department, Subnet, Open port, New, Monitored and No-custom-name. Every tool result grid and every inventory table has its own filter box. Search terms are ANDed; prefix a term with `-` to exclude it. |
| **Upload names / locations** | **Download Template** (XLSX/CSV, with example rows), fill it in, then **Upload Names / Locations**. Rows are matched by MAC first (survives DHCP changes), then by IP. Sets Host Name, Location, Department, Owner, Asset Tag, Type, Notes and monitoring. A copy of the template ships in `dist\`. |
| **Monitoring** | Checks devices by Ping, TCP port or HTTP URL at a set interval. Shows status, latency, avg/min/max, packet loss %, uptime %, a trend sparkline and a history chart. Alerts on DOWN/UP and high latency with sound and Windows notifications. Can minimize to the system tray. |
| **Network tools** | Continuous ping (with chart and jitter), traceroute, port scanner (presets up to 1-65535, banners), DNS lookup (A/AAAA/MX/NS/TXT/SOA/SRV/CAA/PTR, custom server), Whois, Wake-on-LAN, subnet calculator and splitter, ARP table (flush, add to inventory), connections/netstat with owning process, interface live bandwidth, HTTP/SSL certificate check, SNMP walk/get browser. |
| **Device actions** | Right-click any device: Details, Edit, Ping, Trace, Port scan, SNMP, WOL, RDP, SSH, Telnet, Web, `\\shares`, Computer Management, Copy, Monitor, Rescan, Collect inventory. |
| **Reports** | Export the filtered device list to XLSX, CSV or a styled HTML report. Export a per-device inventory report as HTML. Export the event log. |
| **Event log** | Discovery of new devices, IP/MAC changes, up/down events, scans and imports, with severity, source, period and text filters. |

## Notes
- Data is stored in `%AppData%\NetSentinel` (devices, settings, events). The WMI password is encrypted with Windows DPAPI.
- Remote Windows inventory needs an admin account on the target (**Settings → Remote Inventory**, or per device in Details), and WMI allowed through the target's firewall.
- SNMP needs v1/v2c enabled on the device and the community string set in Settings.
- Switch serial numbers and MAC tables come from SNMP. A smart switch with SNMP off shows its model (from its web page or NSDP) but not its serial. An unmanaged switch's own MAC, model and serial can never be read over the network.
- Vendor names: a built-in table covers common brands. Use **Settings → Download full IEEE vendor list** for complete coverage.
- Run as Administrator to flush the ARP cache and for the fullest local WMI data.
