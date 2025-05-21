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

Option 1: Check router DHCP leases
Option 2: Run on Pi:
```bash
ip a
```
3. SSH into Raspberry Pi
   
Terminal or Putty

ssh pi@[IP_ADDRESS]

Default password: (what you set in Imager)

4. Update System

```bash
sudo apt update && sudo apt upgrade -y
```
5. Set Static IP (Recommended)
   
Option 1: DHCP Reservation (Best)

Reserve IP in router settings (This is what I did)

```bash
sudo nano /etc/dhcpcd.conf
```
Add

```bash
interface eth0
static ip_address=192.168.1.100/24
static routers=192.168.1.1
static domain_name_servers=192.168.1.1
```

Then:

```bash
sudo reboot
```
6. Install Pi-hole

```bash
curl -sSL https://install.pi-hole.net | bash
```
Follow prompts:

Confirm static IP

Select upstream DNS (temporary)

Install web admin

Enable query logging

You will get a passwrod at the end of Installation

7. Access Web Interface

On web browser

http://[PI_IP]/admin

8. Add Blocklists (Suggested)

Go to Admin -> Adlists

Add lists from [The Firebog](https://firebog.net/) or [hagezi](https://github.com/hagezi/dns-blocklists?tab=readme-ov-file)

Update Gravity







