## Next actions
1. Physically set up hardware:
   - Desktop PC (x86_64) as K3s control-plane
   - Two Raspberry Pi nodes as workers
   - Ensure stable power, networking, and cooling

2. Assign and confirm network identities:
   - Static DHCP leases or static IPs for all nodes
   - Hostnames resolvable locally (DNS or /etc/hosts)

3. Create local Ansible inventory:
   - Populate `bootstrap/inventory.ini` with real IPs / hostnames

4. Write first Ansible playbook:
   - SSH connectivity check
   - Basic OS sanity (users, packages, time sync)
   - No Kubernetes yet

Do not add Kubernetes manifests until Ansible connectivity is proven.
