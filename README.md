<a id="readme-top"></a>
## About The Project
BoardNet is a CSE421(Computer Networks) course project for BRAC University made in Cisco Packet Tracer that links six education boards across Bangladesh into a unified network for sharing files, emails, and websites. Built with real-world networking techniques like DHCP, DNS, RIP routing, web services, and email communication, it includes automatic backup paths to keep communication seamless even if a connection fails.
<p align="right">(<a href="#readme-top">back to top</a>)</p>

# Built With
* [![Packet Tracer](https://img.shields.io/badge/Cisco-Packet_Tracer-005073?style=for-the-badge&logo=cisco&logoColor=white)](https://www.netacad.com/courses/packet-tracer)
* [![Cisco Networking](https://img.shields.io/badge/Cisco-Networking-1BA0D7?style=for-the-badge&logo=cisco&logoColor=white)](https://www.cisco.com/)
<p align="right">(<a href="#readme-top">back to top</a>)</p>

# Project Overview
### Key Features
- VLSM subnetting from base network `16.65.0.0/16`
- RIPv2 routing between DHK, SYL, and RAJ
- Static routing for KHU and BAR
- Backup paths (floating static routes) for KHU and BAR
- Default route on CTG (only connected to DHK)
- DHCP from DHK router for DHK, CTG, RAJ, and SYL
- Local DHCP servers for KHU and BAR
- Central DNS server in DHK
- Two websites: `www.dhk.edu.bd` and `www.ctg.edu.bd`
- Email server in every board (`mail.dhk.edu.bd`, etc.)
### Network Layout
| Board | Network        | Hosts |
|-------|----------------|-------|
| DHK   | 16.65.0.0/23   | 300   |
| CTG   | 16.65.2.0/24   | 200   |
| RAJ   | 16.65.3.0/24   | 160   |
| SYL   | 16.65.4.0/24   | 150   |
| BAR   | 16.65.5.0/25   | 120   |
| KHU   | 16.65.5.128/25 | 120   |
### Project Files
- `project.pkt`: the Packet Tracer file
- `report.pdf`: full report (VLSM tree, IP table, router configs, services)
<p align="right">(<a href="#readme-top">back to top</a>)</p>

# How To Run
1. Install Cisco Packet Tracer
2. Open `project.pkt`
3. Wait a few seconds for all links to turn green
<p align="right">(<a href="#readme-top">back to top</a>)</p>



# Testing Connectivity (Ping)
Open the **Command Prompt** on any PC and run:
```
ping 16.65.0.1
```
To test cross-board connectivity, ping a PC in a different board:
```
# From a DHK PC to a CTG PC
ping 16.65.2.2

# From a DHK PC to a RAJ PC
ping 16.65.3.2

# From a DHK PC to a SYL PC
ping 16.65.4.2

# From a DHK PC to a KHU PC
ping 16.65.5.130

# From a DHK PC to a BAR PC
ping 16.65.5.2
```
<p align="right">(<a href="#readme-top">back to top</a>)</p>



# Testing Web Services
1. Open the **Web Browser** on any PC (click the PC → Desktop → Web Browser)
2. Type one of the following URLs in the address bar:
```
www.dhk.edu.bd
www.ctg.edu.bd
```
3. The hosted web page should load successfully from any board's PC

# Testing Email Services
1. Open the **Email** client on a PC (click the PC → Desktop → Email)
2. Configure the email client with the following settings:
   - **Your Name:** (any name)
   - **Email Address:** `user@dhk.edu.bd` (use the board's domain)
   - **Incoming Mail Server:** `mail.dhk.edu.bd` (use the board's mail server)
   - **Outgoing Mail Server:** `mail.dhk.edu.bd` (same as incoming)
   - **Username & Password:** (as configured on the mail server)
3. Compose and send an email to a user in another board, e.g., `user@ctg.edu.bd`
4. Open the recipient PC's email client and click **Receive** to verify the email arrived

# Testing DNS Resolution
Open the **Command Prompt** on any PC and use `nslookup` to verify DNS:
```
nslookup www.dhk.edu.bd
nslookup www.ctg.edu.bd
nslookup mail.dhk.edu.bd
nslookup mail.ctg.edu.bd
```
Each query should return the correct IP address resolved by the central DNS server in DHK.
<p align="right">(<a href="#readme-top">back to top</a>)</p>


# Testing Route Failover (Backup Paths)
To verify that floating static routes work for KHU and BAR:

1. **Check current route** — on the KHU or BAR router, open the CLI and run:
   ```
   show ip route
   ```
   Note the primary static route entry.

2. **Simulate a link failure** — delete the primary static route on the router:
   ```
   enable
   configure terminal
   no ip route <destination-network> <subnet-mask> <primary-next-hop>
   exit
   ```
   Alternatively, shut down the primary link interface:
   ```
   enable
   configure terminal
   interface <primary-interface>
   shutdown
   exit
   ```

3. **Verify failover** — run `show ip route` again to confirm the backup (floating static) route has taken over, then ping from a KHU or BAR PC to verify connectivity still works:
   ```
   ping 16.65.0.2
   ```

4. **Restore the primary link** — bring the interface back up or re-add the static route:
   ```
   enable
   configure terminal
   interface <primary-interface>
   no shutdown
   exit
   ```
   Run `show ip route` once more to confirm the primary route is restored.

<p align="right">(<a href="#readme-top">back to top</a>)</p>





