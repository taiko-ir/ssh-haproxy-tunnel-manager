# SSH + HAProxy Tunnel Manager (autossh + systemd)

A small Bash tool to manage multiple **autossh** TCP tunnels on a gateway server and expose them through **HAProxy**.

## Why this is different
Most SSH tunnel scripts stop at port-forwarding.
This project integrates **HAProxy TCP frontends/backends** so you get:
- One public TCP port per service on the gateway
- Load-balancing across multiple parallel autossh forwards
- Basic health-checking (tcp-check) and better resilience
- No need for HAProxy include/cfgdir (works with older HAProxy configurations)

## Features
- Add a tunnel (creates config, systemd units, HAProxy entries)
- Edit a tunnel (updates destination/ports and restarts safely)
- Show status (per tunnel, per autossh instance)
- Managed HAProxy block auto-generated between markers in `/etc/haproxy/haproxy.cfg`

## Requirements
- autossh
- haproxy
- systemd
- iproute2 (ss)

## Quick install
```bash
sudo cp tunnel-manager.sh /usr/local/sbin/tunnel-manager.sh
sudo chmod +x /usr/local/sbin/tunnel-manager.sh
sudo /usr/local/sbin/tunnel-manager.sh

## Notes / Safety
- The script writes to /etc/ssh-tunnel-manager/ and /etc/haproxy/haproxy.cfg.
- Do not commit your real tunnel .conf files to GitHub (they may contain sensitive IPs/ports).
