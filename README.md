# Remote Access with Termix

A university networking project implementing **Termix**, a self-hosted, web-based remote access and server management platform, deployed on a segmented **Proxmox** virtualization environment.

## Overview

This project demonstrates secure remote access to virtual machines using multiple protocols (SSH, VNC, Telnet), managed through a single web dashboard (Termix), and delivered over a properly segmented network protected by a dedicated router.

## Project Components

- **Termix** — LXC container running the web dashboard (nginx, guacd, sshd) used to manage and access remote hosts through the browser.
- **Kali Linux VM** — target host used to test and demonstrate SSH, VNC, and Telnet connections through Termix.
- **OPNsense Router VM** — dedicated router VM providing network segmentation, with WAN on `vmbr0` (university network) and LAN on `vmbr1` (isolated internal network).
- **NAT / Port Forwarding** — configured on OPNsense to forward external traffic to internal hosts, demonstrating a realistic gateway setup.
- **zrok Tunnel** — reverse tunneling service used to expose the Termix dashboard with a stable public URL, allowing access from outside the university network.

## Repository Structure

Remote-Access-with-Termix/
├── README.md
├── Report/
│ └── proxmox_project_final0.pdf
├── Slides/
│ └── termix_presentation.pptx
└── Commands-and-Configs.md


## Contents

- **Report/** — Full project report covering objectives, environment setup, protocol testing (SSH, VNC, Telnet), router configuration, and secure remote access.
- **Slides/** — Presentation slides summarizing the project for the oral defense.
- **Commands-and-Configs.md** — Key commands and configuration values used throughout the project (protocol setup, NAT rules, VNC, zrok tunnel).

## Author

Haneen Baradia
