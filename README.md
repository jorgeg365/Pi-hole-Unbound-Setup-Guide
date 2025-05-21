# Pi-hole + Unbound Setup on Raspberry Pi

## PREREQUISITES
- Raspberry Pi (3B+/4/5 recommended)
- 8GB+ microSD card
- Ethernet connection (recommended)
- Raspberry Pi Imager (https://www.raspberrypi.com/software/)

## INSTALLATION STEPS

1. FLASH RASPBERRY PI OS LITE
- Use Raspberry Pi Imager to install:
  - OS: Raspberry Pi OS Lite (64-bit)
- Configure settings:
  - Hostname: pihole.local
  - Enable SSH
  - Set username/password
  - (Optional) Configure Wi-Fi

2. FIND RASPBERRY PI IP
Option 1: Check router DHCP leases
Option 2: Run on Pi:
ip a

3. SSH INTO RASPBERRY PI
ssh pi@[IP_ADDRESS]
Default password: (what you set in Imager)

4. UPDATE SYSTEM
sudo apt update && sudo apt upgrade -y

5. SET STATIC IP (RECOMMENDED)
Option 1: DHCP Reservation (Best)
- Reserve IP in router settings

Option 2: Manual Static IP
sudo nano /etc/dhcpcd.conf
Add:
interface eth0
static ip_address=192.168.1.100/24
static routers=192.168.1.1
static domain_name_servers=192.168.1.1
Then:
sudo reboot

6. INSTALL PI-HOLE
curl -sSL https://install.pi-hole.net | bash
Follow prompts:
- Confirm static IP
- Select upstream DNS (temporary)
- Install web admin
- Enable query logging

Set admin password:
pihole -a -p

7. ACCESS WEB INTERFACE
http://[PI_IP]/admin

8. ADD BLOCKLISTS (OPTIONAL)
1. Go to Admin -> Adlists
2. Add lists from The Firebog (https://firebog.net/)
3. Update Gravity

9. CONFIGURE NETWORK DNS
Option A: Manual per device
- Set DNS to Pi-hole IP

Option B: Router DHCP (Recommended)
- Set primary DNS = Pi-hole IP
- Secondary = Backup Pi-hole or 1.1.1.1

## INSTALL UNBOUND (RECURSIVE DNS)

1. INSTALL UNBOUND
sudo apt install unbound -y

2. CONFIGURE UNBOUND
sudo nano /etc/unbound/unbound.conf.d/pi-hole.conf
Paste:
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

3. RESTART UNBOUND
sudo service unbound restart

4. TEST UNBOUND
dig example.com @127.0.0.1 -p 5335

5. CONFIGURE PI-HOLE TO USE UNBOUND
1. Go to Settings -> DNS
2. Remove all upstream servers
3. Add custom upstream: 127.0.0.1#5335

## VERIFICATION
- Test ad blocking: https://ads-test.pi-hole.net
- Check dashboard for blocked queries

## MAINTENANCE
Update Pi-hole:
sudo pihole -up

## LICENSE
MIT
