# Pi-hole + Unbound Setup on Raspberry Pi

A complete guide to setting up **Pi-hole** (network-wide ad blocker) and **Unbound** (recursive DNS resolver) on a Raspberry Pi.

## 📋 Prerequisites
- Raspberry Pi (3B+/4/5 recommended)
- 8GB+ microSD card
- Ethernet connection (recommended)
- [Raspberry Pi Imager](https://www.raspberrypi.com/software/)

---

## 🚀 Installation Steps

### 1. Flash Raspberry Pi OS Lite

 Use Raspberry Pi Imager to install:
 OS: Raspberry Pi OS Lite (64-bit)
 Configure settings:
 - Hostname: pihole.local
 - Enable SSH
 - Set username/password
 - (Optional) Configure Wi-Fi

2. Find Raspberry Pi IP

# Option 1: Check router DHCP leases
# Option 2: Run on Pi:
```bash
ip a
```
3. SSH into Raspberry Pi
# Terminal or Putty
ssh pi@[IP_ADDRESS]
# Default password: (what you set in Imager)
