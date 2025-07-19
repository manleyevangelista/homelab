# TrueNAS Server Configuration

## Storage

* Storage: `256GB SSD NVMe x 1`
* Type: `Stripped`
* Pool name: `apps_vm_ssd_pciex4nvme`
* Purpose: `applications and virtual machines`

* Storage: `4TB HDD SATA x 2`
* Type: `Mirrored`
* Pool name: `raid1_hdd`
* Purpose: `storage and file sharing`

## Datasets

* Name: `files`
* Location: `raid1_hdd`
* Type: `FILESYSTEM`
* Sync: `STANDARD`
* Compression: `Inherit (1.00x (LZ4))`
* Enable Atime: `OFF`
* ZFS Deduplication: `OFF`
* Case Sensitivity: `ON`
* Path: `raid1_hdd/files`

## Shares

Windows (SMB) Shares

* Path: `/mnt/raid1_hdd/files`
* Name: `files'
* Purpose: `Default share paramenters`
* Description:
* Enabled: `[x]`

## Data Protection

### Scrub Tasks

* Pool: `raid1_hdd`
* Threshold Days*: `35`
* Description:
* Schedule: `Weekly (0 0 * * sun) On Sundays at 00:00 (12:00 AM)`
* Enabled: `[x]`

* Pool: `apps_vm_ssd_pciex4nvme`
* Threshold Days*: `35`
* Description:
* Schedule: `Weekley (0 0 * * sun) On Sundays at 00:00 (12:00 AM)`
* Enabled: `[x]`

## Periodic Snapshot Tasks

* Dataset*: 'raid1_hdd"
* Exclude:
* Recursive" `[x]`
* Snapshot Lifetime8*: `1 WEEK`
* Naming Scheme: `auto-%Y-%m-%d_%H-%M`
* Schedule*: `Hourly (8****) At the start of each hour`
* Begin*: `00:00:00`
* End*: `23:59:00`
* Allow Taking Empty Snapshots: `[x]`
* Enabled: `[x]`

## Credentials

### Interfaces

* Name*: `eno1`
* Description: `Onboard Ethernet port`
* DHCP: `[ ]`
* Autoconfigure IPv6: `[ ]`
* MTU: `1500`
* Aliases:

* Name*: `br0`
* Description: `Onboard Ethernet port.`
* DHCP: `[ ]`
* Autoconfigure IPv6: `[ ]`
* Bridge Members: `eno1`
* Enable Learning: `[x]`
* MTU: 1500
* Aliases:
  * IP Address*: `192.168.0.3/24`

* Name*: `zth6rot23g`
* Description: `ZeroTier interface.`
* DHCP: `[ ]`
* Autoconfigure IPv6: `[ ]`
* MTU: `1500`
* Aliases:


## Global Configuration

### Hostname and Domain

* Hostname: `truenas-hp800g3`
* [x] Inherit odmain from DHCP
* Domain:
* Additional Domains:

### Service Announcement

* [x] NetBIOS-NS
* [x] mDNS
* [x] WS-Discovery

### DNS Servers

* Nameserver #1: `192.158.0.1`
* Nameserver #2:
* Nameserver #3:

### Default Gateway

* IPv4 Default Gateway: `192.168.0.1`
* IPv6 Default Gateway:

### Outbound Network

* [x] Allow All

### Other Settings

* HTTP Proxy:
* Host Name Database:

## Credentials

### Users

#### root

**Identification**

* Full Name*: `root`
* Disable Password: `Disable`
* E-Mail:
* New Password:
* Confirm New Password:

**User ID and Groups**

* UID*: `0`
* Auxillary Groups: `builtin_administrators`
* Create New Primary Group: `Disabled`
* Primary Group: `wheel`

**Directories and Permissions**

* Home Directory: `/root`

Home Directory Permissions:
* User: `Read, Write, Execute`
* Group:
* Other: 
* [ ] Create Home Directory

**Authentication**

* Authentication Keys:
* [x] SSH password login enabled
* Shell*: `zsh`
* [x] Lock User
* Allowed sudo commands:
* [ ] Allowed all sudo commands
* Allowed sudo commands with password:
* [ ] Allowed all sudo commands with no password
* [ ] SMB User

#### manley

**Identification**

* Full Name*: `manley`
* Disable Password: `Disable`
* E-Mail:
* New Password:
* Confirm New Password:

**User ID and Groups**

* UID*: `3000`
* Auxillary Groups: `builtin_users; ftp`
* Create New Primary Group: `Disabled`
* Primary Group: `manley`

**Directories and Permissions**

* Home Directory: `/var/empty`

Home Directory Permissions:
* User: `Read, Write, Execute`
* Group:
* Other: 
* [ ] Create Home Directory

**Authentication**

* Authentication Keys:
* [x] SSH password login enabled
* Shell*: `nologin`
* [x] Lock User
* Allowed sudo commands:
* [ ] Allowed all sudo commands
* Allowed sudo commands with password:
* [ ] Allowed all sudo commands with no password
* [x] SMB Useer

#### <moms_name>

**Identification**

* Full Name*: `<moms_name`
* Disable Password: `Disable`
* E-Mail:
* New Password:
* Confirm New Password:

**User ID and Groups**

* UID*: `3001`
* Auxillary Groups: `builtin_users`
* Create New Primary Group: `Disabled`
* Primary Group: `<moms_name`

**Directories and Permissions**

* Home Directory: `/var/empty`

Home Directory Permissions:
* User: `Read, Write, Execute`
* Group:
* Other: 
* [ ] Create Home Directory

**Authentication**

* Authentication Keys:
* [] SSH password login enabled
* Shell*: `nologin`
* [x] Lock User
* Allowed sudo commands:
* [ ] Allowed all sudo commands
* Allowed sudo commands with password:
* [ ] Allowed all sudo commands with no password
* [x] SMB Useer

### Groups

manley

* GID*: `3000`
* Name*: `manley`
* Priviledges:
* Allowed sudo commands:
* [ ] Allowed all sudo commands
* Allowed sudo commands with password:
* [ ] Allowed all sudo commands with no password

<moms_name>

* GID*: `3000`
* Name*: `<moms_name>`
* Priviledges:
* Allowed sudo commands:
* [ ] Allowed all sudo commands
* Allowed sudo commands with password:
* [ ] Allowed all sudo commands with no password

## Instances

Name: `Windows`

**Instance Configuration** 

* [x] Autostart

**CPU & Memory** 

* CPU Configuration*: `4`
* Memory Size*: `8 GiB`

### VNC

* [x] Enable VNC
* VNC Port: 5900
* VNC Password:

# Security

* [x] Secure Boot

### Devices

* Trusted Platform Module (TPM)
* USB: xHCI Host Controller (1d6b:0003)
* USB: xHCI Host Controller (1d6b:0002)

### NIC

* NIC: eth0 (BRIDGED)

### Disks

* Root Disk: `160 GiB (NVMe)`
* Location: `apps_vm_ssd_pciex4nvme`

## Apps

### Jellyfin

**Additonal Storage**

* Type*: `Host Path (Path that already exists on the system)`
* Mount Path*: `/files`
* Host Path*: `/mnt/raid1_hdd/files`

**Resource Configuration**

* CPUs*: `2`
* Memory (in MB)*: `8192`
* GPU Configuration: `[x] Passthrough available (non-NVIDIA) GPUs`

**Transcoding**

* Hardware acceleration: `Intel QuickSync (QSV)`
* QSV Device: `/dev/dri/renderD128`
* Enable hardware decoding for: `H264, HEVC, MPEG2, VC1, VP8, VP9, HEVC 10bit, VP9 10bit, Prefer OS DXVA or VA-API hardware decoders`
* Hardware encoding options: `Enable hardware encoding`

### Tailscale

**Tailscale Configuration**

* Hostname*: `truenas-hp800g3`
* Auth key*: `Get it from Tailscale.`
* [x] Auth Once
* [ ] Reset
* [x] Accept DNS
* [ ] Userspace
* [x] Advertise Exit Node
*Advertise Routes (Route*): `192.168.0.3/32`

**Network Configuration**

* [x] Host Network

### Zerotier

**Zerotier Configuration**

* Network* (Network ID*): `Get it from ZeroTier`
* Auth key*: `Get it from Tailscale.`

**Network Configuration**

* [x] Host Network

## System

### Advanced Settings

* Description: `IP forward from Zerotier Interface to Onboard Ethernet.`
* Command: `iptables -A FORWARD -i zth6rot23g -o eno1 -j ACCEPT`
8 Type*: `Command`
* When*: `Post Init`
* [x] Enabled
* Timeout: `10`


* Description: `IP forward from Onboard Ethernet  Zerotier Interface.`
* Command: `iptables -A FORWARD -i eno1 -o zth6rot23g -j ACCEPT`
8 Type*: `Command`
* When*: `Post Init`
* [x] Enabled
* Timeout: `10`
