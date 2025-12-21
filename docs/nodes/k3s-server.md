# K3s Server (Control Plane Node)

## Identity
- Hostname: k3s-server
- Role: K3s server / control plane
- Architecture: x86_64
- OS: Ubuntu Server 24.04 LTS

## Networking
- IP: 192.168.0.210
- DHCP: Static lease
- DNS: Local (client-side hosts entry)

## Access
- SSH: key-based only
- Password auth: disabled
- Root login: disabled

## Boot
- Headless auto-boot
- No GRUB interaction required
- Monitor/keyboard not required

## Time
- NTP enabled
- Clock synchronized

## Status
- Known-good baseline
- No Kubernetes installed yet
- Safe starting point for Ansible bootstrap
