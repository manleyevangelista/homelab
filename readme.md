# Manley's Mini Home Lab
<img src="https://github.com/manleyevangelista/homelab/blob/main/images/07172025/homelab_setup_07172025.jpg" style="width:800px;">

## Intro
This repository is dedicated to my homelab, which serves as the core of my home network setup. It currently includes three routers and one compute device functioning as a dedicated server.

In the sections below, I’ll break down the hardware, software, and strategies I use to keep it up and running.

## Rationale

I wanted something simple, silent, and reliable, with some level of redundancy — especially for the server, since it doesn’t just house my files, but actively handles them on a daily basis.

## Hardware
<img src="https://github.com/manleyevangelista/homelab/blob/main/images/07172025/Network%20Map.jpg" style="width:800px;">

### Routers

**Note**: All these only have a gigabit port. 

**Router #1**: Fiberhome HG6145D (Globe GFiber Prepaid)

This is our primary internet connection. So far, it’s been rock stable — rain or shine. DMZ is configured to point to Router #3.

WiFi is set to Channel 6 (2.4 GHz) and Channel 140 (5 GHz) to avoid interference with Router #3. The SSIDs are also hidden to prevent confusion when guests try to connect. If a technician needs to connect, I can show them a QR code anyway.

**Router #2**: Zowee H155-382 (PLDT Home WiFi Prepaid)

This is our backup internet connection. While our primary connection is rock stable, sometimes the fiber line outside breaks and needs repair. While waiting, this connection takes over. The beauty of it — it's prepaid. We only have to pay when we actually need to use it. DMZ is also configured to point to Router #3.

WiFi is set to automatic on both 2.4 GHz and 5 GHz bands, since this router can be used anywhere—whether in a car, at the park, or anywhere with a 12V power source and power bank. The SSIDs are also hidden to prevent confusion when guests try to connect. Whenever guests needs to connect, I can show them a QR code anyway.

**Router #3**: TP-Link AX3000

Now, this is the main hub for both my homelab and home network. Either Router #1 or #2 is connected to its WAN port, depending on which internet connection I’m using at the moment. From here, everything else connects — the server, a repeater in the garage, and all WiFi devices.

WiFi is set to Channel 11 (2.4 GHz) and Channel 165 (5 GHz) to avoid interference with Router #1.

<img src="https://github.com/manleyevangelista/homelab/blob/main/images/07172025/MACAddressBinding%202025-07-17%20172239.png" style="width:800px;">

On this router, I’ve set the DNS to AdGuard’s public DNS, which blocks ads network-wide. I’ve also bound the MAC addresses of the server and CCTV cameras to static IPs — makes them easier to reference.

The reason I have separate routers is simple: if one ISP goes down, I don’t need to spend half a day reconnecting a dozen WiFi devices. Not to mention, not long ago, our ISP swapped Router #1 with a new one — more locked down and with fewer Ethernet ports. I can’t even change the DNS. At least that won’t be a problem with this router.

### Server
<p align="left">
  <img src="https://github.com/manleyevangelista/homelab/blob/main/images/07172025/hp_elitedesk800g3_front.jpg" style="width:500px;">
  <img src="https://github.com/manleyevangelista/homelab/blob/main/images/07172025/hp_elitedesk800g3_back_07172025.jpg" style="width:500px;">
</p>

**HP EliteDesk 800 G3**

Specs: 
- Intel Core i5-7500 3.4GHz (4 Cores, 4 Threads)
- 32GB (4x8GB) DDR4 RAM (2x SK-Hynix + 2x Kingston HyperX Fury)
- 128GB SK-Hynix NVMe SSD (built-in M.2 slot on motherboard) - TrueNAS Boot
- 256GB SK-Hynix NVMe SSD (PCI-E slot via PCI-E to M.2 adapter) - Apps and VM Pool
- 2x4TB Seagate SkyHawks - Storage Pool
- 180W Power Supply 80-Plus Bronze
- 1x Gigabit Ethernet
- 9x USB-A ports (6x USB 3; 4x USB 2)
- 1x USB-C port (USB 3)
- 2x DisplayPorts
- 1x PCI-E X16
- 1x PCI-E X4
- 2X PCI-E X2

## Software
All of the routers run their respective stock firmware — none of them have custom firmware installed. For one, there’s no custom firmware available for them anyway. And second, I don’t want to risk bricking anything, because if I screw it up, I’ll have to pay someone from the ISP to come and fix it. Especially with the Globe router — if it gets reset, all of its settings (including the IPOE credentials) are wiped, and Globe usually won’t give those out.

The server, by the way, is running TrueNAS Scale 24.04.1 as of writing. The following things have been set, not limited to:

## Configuration
<p align="left">
    <img src="https://github.com/manleyevangelista/homelab/blob/main/images/07172025/hp_elitedesk800g3_openbirdseyeview_07172025.jpg" style="width:495px;">
    <img src="https://github.com/manleyevangelista/homelab/blob/main/images/07172025/hp_elitedesk800g3_openangled_07172025.jpg" style="width:500px;">
</p>


- The two hard drives (2×4TB Seagate SkyHawks) are configured as **RAID 1** pool.
  - Yes, RAID 1 is not a backup, but it does provide a layer of protection in case one of the drives fails, since data is mirrored to both. It's unlikely for both drives to fail at the same time—so if one dies, I just replace it, and the system rebuilds the mirror, like nothing happened.
- Created a single dataset and network share to house all files in the storage pool.
   - This simplifies file management and makes it easier to keep track of and move files around.
- I have a **network bridge** (br0) configured, so virtual machines can access the server.
- There's a **Windows 11 VM** installed. It's mainly used to sync the only network share (which includes all files in the storage pool) to an NTFS-formatted external drive. TrueNAS itself can’t write to NTFS, so this workaround handles that.
  - It's not limited to that—it also doubles as a cloud desktop when I'm away from home—handy for accessing Microsoft Office, Adobe Photoshop, or for browsing with a Philippine IP when needed.
- IP forwarding is enabled for Tailscale and ZeroTier, allowing me to access the server using the same local IP—whether I’m at home or connected remotely via VPN.
- The storage pool is scrubbed every two weeks, which basically checks for any errors and fixes whatever needs fixing.
- I also have automatic snapshots taken weekly.
   - That way, if something gets accidentally deleted or goes missing, I can recover it easily without having to fetch the external backup drive.
- Installed and configured both Tailscale and ZeroTier to enable secure **remote access** outside the house.
   - I was able to access my server halfway across the world (I was in America; the server is in the Philippines).
   - I was even able to access it on the plane—35,000 feet above the middle of the Pacific Ocean.
- Installed Jellyfin to act as a **media server**.
  - It’s configured to use Intel Quick Sync for hardware-accelerated encoding, allowing smooth media streaming to devices that may not support certain codecs.

## Backup strategy
<p align="left">
    <img src="https://github.com/manleyevangelista/homelab/blob/main/images/07172025/20250717_1BackupToExtHDD.jpg" style="width:500px;">
</p>


While RAID can provide some level of redundancy, it’s not a true backup. It doesn’t protect against file corruption, human error (like accidental deletion or overwriting), or catastrophic events such as theft or natural disasters. That’s why an additional layer of protection is essential: regularly backing up all files from the storage pool to an external drive, and storing that drive offsite.

<p align="left">
    <img src="https://github.com/manleyevangelista/homelab/blob/main/images/07172025/PassExtHJDD.png" style="width:500px;">
    <img src="https://github.com/manleyevangelista/homelab/blob/main/images/07172025/ExtHDDOnVM.png" style="width:378px;">
</p>

To implement this, I mounted the network share (which is the only file share on the server) as a mapped drive inside a Windows 11 virtual machine. I configured the VM to accept USB devices and passed through the connected external drive. Once recognized by Windows, I used a free program called FreeFileSync to mirror the contents of the network share onto the external drive. After syncing, I simply eject the drive from Windows, unplug it, and take it offsite for safekeeping.

<p align="left">
    <img src="https://github.com/manleyevangelista/homelab/blob/main/images/07172025/FreeFileSync_Syncing.png" style="width:500px;">
</p>

In the event something goes wrong—or if something happens to me—my family can still access the files with ease. They can just plug the external drive into any computer and view the contents without needing special software or technical knowledge.

(Yes. At the moment, I'm using a 2TB external drive to back up my 4TB server. My total data usage isn't even close to 2TB, so I'm just reusing the drive that used to store my files. If I eventually get closer to 2TB, I'll replace it with a 4TB one.)

<img src="https://github.com/manleyevangelista/homelab/blob/main/images/07172025/ExtHDDNTFS_2025-07-17%20.png" style="width:400px;">

I resorted to this workaround because TrueNAS doesn’t natively support backing up directly to an NTFS- or FAT-formatted drive. In fact, there’s no built-in, one-click solution for external drive backups.

The closest native option would involve creating a ZFS pool on the external drive, setting up a Replication Task to copy datasets from the main storage pool, and then exporting the ZFS pool after transfer. But this makes the external drive inaccessible on most systems—only Linux with ZFS utilities installed can recognize it, and on top of that, it requires entering a command (in terminal) to mount. That’s too convoluted for other family members to do.

## Costs

| Item                                                         | Cost (in USD) | Source                              |
|:-------------------------------------------------------------|:--------------|:------------------------------------|
| Fiberhome HG6145D (Globe GFiber Prepaid)                     | $26.50        | Globe Telecom (Online; Philippines) |
| Zowee H155-382 (PLDT Home WiFi 5G)                           | $26.68        | PLDT Store (SM; Philippines)        |
| TP-Link Archer AX3000                                        | $5.99         | Savers (Thrift Store; U.S.)         |
| HP EliteDesk 800 G3 (i5-7500, 16GB RAM, 1TB HDD, Win 11 COA) | $97.30        | Facebook Marketplace (Philippines)  |
| 2x4TB Seagate SkyHawk(s)                                     | $180.26       | Lazada (Philippines)                |
| 2x8GB Kingston HyperX Fury 2666MHz                           | $22.38        | Shopee (Philippines)                |
| 128GB SK-Hynix NVMe SSD                                      | $10.60        | Shopee (Philippines)                |
| 256GB SK-Hynix NVMe SSD                                      | $16           | Facebook Marketplace (Philippines)  |
| SATA cable (for second HDD)                                  | $0.01         | Shopee (Philippines)                |
| Set of HP drive screws w/ gromets (for second HDD)           | $2.00         | Lazada (Philippines)                |
| PCIe to NVMe adapter (for second SSD)                        | $2.75         | Lazada (Philippines)                |
| **Total**                                                    | **$390.47**   |                                     |

**Note**: Total is approximate, due to varying conversion rates. 

## Closing

I’m happy with my current setup, and it should last me at least the next five years (as of July 2025). I love how silent the whole thing is. I didn’t go with WiFi 7 or 2.5Gbps Ethernet because only one device supports it, so it’s not worth the extra cost.

I also chose 4TB of storage since I don’t have many files. Out of the 1.3TB I’m using, about half are movies and shows that I delete after watching. I’ll upgrade storage only when needed.

Overall, this setup balances between cost and practicality. 
