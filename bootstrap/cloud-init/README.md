# Headless Pi bootstrap (Ubuntu Server + cloud-init)

## Files
- user-data.template.yaml
- network-config.yaml

## One-time laptop prerequisites
- SSH key exists at `~/.ssh/id_ed25519.pub`
- Router will provide DHCP (prefer DHCP reservation per-node)

## Provision procedure (NVMe/SSD)
1. Flash Ubuntu Server (Raspberry Pi image) to SSD/NVMe.
2. Mount the SSD boot partition (usually labelled `system-boot`).
3. Create `/system-boot/user-data` from `user-data.template.yaml`:
   - replace `__HOSTNAME__`
   - replace `__SSH_PUBLIC_KEY__` with contents of `~/.ssh/id_ed25519.pub`
4. Copy `network-config.yaml` to `/system-boot/network-config` (verbatim).
5. Safely eject SSD.
6. Boot Pi via Ethernet.
7. Find IP via `arp-scan --localnet` and SSH in:
   - `ssh tom@<ip>`
8. After first boot, set router DHCP reservation using the Pi MAC address.
