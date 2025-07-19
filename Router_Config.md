# Router Configurations

**Note**: This isn't meant to be imported or feed into anything. This is just for reference only.
Note #2: Unless mentioned, everything else are set to default.

## Connections

**Router 1 - FiberHome HG6145D (Globe GFiber Prepaid)**

* LAN #1: `Router 3 - TP-Link AX3000`

**Router 2 - Zowee H155-382 (PLDT Home WiFi 5G)**

* LAN #1: `Router 3 - TP-Link AX3000`

**Router 3 - TP-Link AX3000**

* WAN: `Router 1 - FiberHome HG6145D (Globe GFiber Prepaid)` or `Router 2 - Zowee H155-382 (PLDT Home WiFi 5G)`
* LAN #1: `HP EliteDesk 800 G3 TrueNAS Server`

## Router 1 - FiberHome HG6145D (Globe GFiber Prepaid)

**Purpose**: Acting as Primary WAN for Router #3.

### Wireless

**2.4GHz**

* Channel: `6`
* SSID Hidden?: `Yes`

**5Ghz**

* Channel: `140`
* SSID Hidden?: `Yes`

### DMZ

IP: `192.168.0.1` (TP-Link AX3000) 

## Router 2 - Zowee H155-382 (PLDT Home WiFi 5G)

**Purpose**: Acting as Secondary WAN for Router #3.

### Wireless

**2.4GHz**

* Channel: `Auto`
* SSID Hidden?: `Yes`

**5Ghz**

* Channel: `Auto`
* SSID Hidden?: `Yes`

### DMZ

IP: `192.168.0.1` (TP-Link AX3000) 

## Router 3 - TP-Link AX3000

**Purpose**: Acting as main router/hub for the homelab or home network.

### DHCP Server

* IP Address Pool: `192.168.0.100`
* Default Gateway: `192.168.0.1`
* Primary DNS: `94.140.14.14 (AdGuard)` Note: Up to date as of July 2025.
* Secondary DNS: `94.140.15.15 (AdGuard)`  Note: Up to date as of July 2025.

### MAC Bindings

| **MAC Address (Redacted)** | **Device**                             | **Static IP Address** |
|:---------------------------|:---------------------------------------|:----------------------|
| 52-XX-XX-XX-4E-FD          | Xiaomi Repeater                        | 192.168.0.2           |
| 00-XX-XX-XX-E2-16          | HP EliteDesk 800 G3 TrueNAS Server     | 192.168.0.3           |
| B4-XX-XX-XX-DC-A3          | Gate CCTV                              | 192.168.0.4           |
| 14-XX-XX-XX-02-C4          | Garage CCTV                            | 192.168.0.5           |
| 48-XX-XX-XX-2C-DA          | Poolside CCTV                          | 192.168.0.6           |
| 14-XX-XX-XX-0B-7C          | Side CCTV                              | 192.168.0.7           |
| 48-XX-XX-XX-35-F9          | Living Room CCTV                       | 192.168.0.8           |
| 5C-XX-XX-XX-00-99          | Mom's Room CCTV                        | 192.168.0.9           |

### Wireless

**2.4GHz**

* Channel: `11`
* SSID Hidden?: `No`
* Airtime Fairness Feature: `Yes`

**5Ghz**

* Channel: `165`
* SSID Hidden?: `No`
* Airtime Fairness Feature: `Yes`
* Multi-User MIMO Feature: `Yes`
