Guide to Install and Set Up Pi-hole with Unbound on Raspberry Pi

This guide will walk you through installing Pi-hole (a network-wide ad blocker) and Unbound (a recursive DNS resolver) on a Raspberry Pi for improved privacy and ad-free browsing.

Requirements
Raspberry Pi (Recommended: Pi 3B+/4/5, but Pi 2/3 also works)

MicroSD Card (8GB minimum, 16GB recommended)

Ethernet connection (Wi-Fi is possible but not recommended for stability)

Raspberry Pi Imager (Download Here)

Power supply & case (optional but recommended)

Step 1: Flash Raspberry Pi OS Lite
Download & Install Raspberry Pi Imager on your computer.

Insert microSD card into your computer.

Open Raspberry Pi Imager:

Choose OS → Raspberry Pi OS (Other) → Raspberry Pi OS Lite (64-bit).

Choose Storage → Select your microSD card.

Click the gear icon (⚙️) to configure settings:

Set hostname: pihole.local (or any name you prefer).

Enable SSH → Use password authentication.

Set username & password (default: pi + your password).

Configure Wi-Fi (optional) or stick with Ethernet.

Set locale (timezone & keyboard layout).

Click "Write" and wait for the process to complete (~3-5 min).

Insert microSD into Raspberry Pi, connect Ethernet, and power it on.

Step 2: Find Your Pi’s IP Address
Once booted, find your Pi’s IP address:

Option 1: Check your router’s DHCP lease table.

Option 2: Connect a monitor/keyboard and run:
