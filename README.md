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

In tools make sure to:

Update Gravity

9. Configure Network DNS
Option A: Manual per device
Set DNS to Pi-hole IP

Option B: Router DHCP 
Set primary DNS = Pi-hole IP

Secondary = Backup Pi-hole or 1.1.1.1

(I did option A as a trial)

🔄 Install Unbound (Recursive DNS)
1. Install Unbound

```bash
sudo apt install unbound -y
```

2. Configure Unbound

```bash
sudo nano /etc/unbound/unbound.conf.d/pi-hole.conf
```

Paste:

```yaml
server:
  verbosity: 0
  interface: 127.0.0.1
  port: 5335
  do-ip4: yes
  do-udp: yes
  do-tcp: yes
  do-ip6: no
  prefer-ip6: no
  harden-glue: yes
  harden-dnssec-stripped: yes
  use-caps-for-id: no
  edns-buffer-size: 1232
  prefetch: yes
  num-threads: 1
  so-rcvbuf: 1m
  private-address: 192.168.0.0/16
  private-address: 169.254.0.0/16
  private-address: 172.16.0.0/12
  private-address: 10.0.0.0/8
```

3. Restart Unbound

```bash
sudo service unbound restart
```

4. Test Unbound

```bash
dig example.com @127.0.0.1 -p 5335
```

5. Configure Pi-hole to Use Unbound

Go to Settings -> DNS

Remove all upstream servers

Add custom upstream: 127.0.0.1#5335

✅ Verification

Test ad blocking: [adblock-tester](https://adblock-tester.com/)

Check dashboard for blocked queries






