# Commands and Configurations — Remote Access with Termix

This file documents the key commands and configuration values used throughout the project, provided as supporting technical material alongside the full report and presentation.

## 1. Termix Protocols — Kali VM Setup

### SSH

Installing and starting the SSH service on Kali:

```bash
sudo apt update
sudo apt install openssh-server -y
sudo systemctl enable ssh
sudo systemctl start ssh
```

Kali was then added as a host inside Termix (IP address + SSH protocol enabled), giving a full terminal session inside the browser.

### VNC

Installing TightVNC Server and setting Fluxbox as the window manager:

```bash
sudo apt update
sudo apt install tightvncserver fluxbox -y
vncserver :1
```

Missing font path issue fix (required for the VNC server to start correctly):

```bash
sudo apt install xfonts-base -y
vncserver -kill :1
vncserver :1
```

Kali was then added as a host inside Termix with the VNC protocol enabled, showing the Kali desktop directly inside the browser.

### Telnet

Installing the Telnet daemon (served through `inetd`):

```bash
sudo apt update
sudo apt install telnetd -y
sudo systemctl enable inetutils-inetd
sudo systemctl start inetutils-inetd
```

The Telnet entry was enabled manually inside `/etc/inetd.conf` (disabled by default), then the service was restarted:

```bash
sudo systemctl restart inetutils-inetd
```

Kali was then added as a host inside Termix with the Telnet protocol enabled, reaching the Kali login prompt and a working command-line session.

## 2. OPNsense Router — NAT / Port Forwarding

**Destination NAT rule** forwarding SSH traffic from WAN to the internal Kali VM:

- Interface: WAN
- Protocol: TCP
- Destination port: 22
- Redirect target IP: `192.168.1.193` (internal Kali VM)
- Redirect target port: 22

**WAN Firewall rule** (required in addition to the NAT rule, since OPNsense defaults to deny):

- Action: Pass
- Interface: WAN
- Protocol: TCP
- Destination: Kali VM (192.168.1.193), port 22

## 3. VNC — Real Desktop Access (x11vnc)

Using `x11vnc` to expose the actual XFCE desktop session (instead of a separate virtual display):

```bash
sudo apt install -y lightdm xfonts-base x11vnc
x11vnc -display :0 -rfbport 5902 -forever -shared
```

Restarting the standard VNC server manually after reboot (does not persist automatically):

```bash
vncserver :1
```

Checking active listening services inside the Termix container:

```bash
ss -tlnp
```

## 4. Public Remote Access — zrok Tunnel

Enabling the environment with the account token:

```bash
zrok enable <token>
```

Reserving a permanent public share (instead of a random URL each run):

```bash
zrok reserve public localhost:80 --unique-name termixhaneen
```

Running the share in the background reliably (required flags to prevent the process from stopping on `SIGTTIN`):

```bash
nohup zrok share reserved termixhaneen --headless < /dev/null > zrok.log 2>&1 & disown
```

Resulting permanent public URL:

```
https://termixhaneen.share.zrok.io
```

## 5. Network Segmentation

- `vmbr0` — WAN bridge, connected to the university network
- `vmbr1` — isolated LAN bridge, created for internal traffic between OPNsense, the Kali VM, and other internal hosts
- OPNsense VM configured with WAN on vmbr0 and LAN on vmbr1, acting as the gateway for all internal devices

---
*All configuration values (IP addresses, tokens, ports) reflect the environment used during development and testing.*
