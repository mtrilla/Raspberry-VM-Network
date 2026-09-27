# Design Decisions

## Overview
This project is at the moment a basic simulation of a network,a server with its services and its respective clients, it will use Virtual Machines, and evereything will be running on a Raspberry 5(8GB), in the future it can evolve towards a larger network,or interconnected LAN networks, cybersecurity oriented and more services.

---

## Goals
The main goal is to **learn**, the technical goals are:

* Creating a LAN Network.
    * Installing a server and working services(DHCP,DNS).
    * Installing many User VMs.
* Manage the limited Resources correctly.
* Document and explain everything in the process

---

## Architecture

| Component | Choice |
|---|---|
| Host OS | Raspberry Pi OS (official) |
| Hypervisor | QEMU/KVM |
| VM Management | virsh (CLI) |
| Server distro | RHEL |
| Client distros | Alpine Linux, Debian Server (minimal), Void Linux |
| DNS | BIND9 |
| DHCP | ISC Kea |

### Hosts

| Host | Role | OS |
|---|---|---|
| server01 | DNS + DHCP | RHEL |
| client01 | User PC | Debian Server (minimal) |
| client02 | User PC | Debian Server (minimal) |
| client03 | User PC | Void Linux |
| client04 | User PC | Alpine Linux |

### Network Topology

Single flat LAN for this phase — no segmentation yet (planned for a future phase).

```
Internet
   |
[Gateway/Router]
   |
[LAN - 192.168.X.0/24]
   |
   +-- RHEL Server (DNS + DHCP)
   +-- Client 1 (Debian)
   +-- Client 2 (Debian)
   +-- Client 3 (Void Linux)
   +-- Client 4 (Alpine)
```

---

## Technology choices

#### Rasberry Pi OS
* Official OS, best hardware support
* Lightweight.

#### QEMU/KVM
* Realism
* Full kernel isolation per VM

#### Virsh (CLI)
* The headless standard

#### RHEL (developer subscription)
* Enterprise-standard distribution used in regulated industries (banking, government, healthcare)
* Free via Red Hat Developer subscription

#### Client OS
* Debian Server (minimal):
    * Most common baseline distro, high compatibility
    * Lightweight.
* Void Linux:
    * Lightweight
* Alpine Linux:
    * Lightweight

#### ISC Kea (DHCP)
* Official successor to isc-dhcp-server, which is now deprecated/end-of-life
* Actively developed and maintained by ISC (same team behind BIND9)
* Increasingly adopted by ISPs and data centers migrating away from the legacy DHCP daemon
* JSON-based configuration, more modern than older DHCP config formats

#### BIND9 (DNS)
* Industry-standard DNS software, used by a large share of the internet's infrastructure (including root DNS servers)
* Developed by ISC (Internet Systems Consortium), the reference organization for DNS
* Supports advanced features (DNSSEC, complex zone files) found in real enterprise/ISP environments
* More complex to configure than lightweight alternatives (dnsmasq), which better demonstrates real DNS administration skills

---

## Trade-offs considered

### Hypervisor: QEMU/KVM vs LXC
- LXC considered for lower resource usage and faster boot times
- QEMU/KVM chosen instead for full kernel isolation, more realistic simulation of independent physical hosts

### Server OS: RHEL vs Rocky/AlmaLinux
- Rocky/AlmaLinux considered as free, no-registration clones, binary-compatible with RHEL
- RHEL chosen instead to work with the actual distribution used in real enterprise environments, at the cost of annual subscription renewal

### DNS/DHCP: BIND9/Kea vs dnsmasq
- dnsmasq considered for its simplicity — single lightweight service covering both DNS and DHCP
- BIND9 + Kea chosen instead for closer alignment with real enterprise/ISP infrastructure, at the cost of more complex configuration

### Client OS diversity vs uniformity
- A single client distro (e.g. all Debian) considered for simplicity and easier troubleshooting
- Mixed distros (Debian, Void, Alpine) chosen instead to better simulate the natural heterogeneity of a real network that has grown over time
