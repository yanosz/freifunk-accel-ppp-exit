# Freifunk-PPtP exit (accel-ppp-based)

This role configures accel-pppd to be used as an PPtP Internet-Exit for Freifunk nodes.

Please note, that this role **does not** install accel-ppp, since no debian packages are provided at any reasonable source. This role doesn't do anything with accel-ppp - except managing `/etc/accel-ppp.conf`.

Also note, that PPTP's encryption is insecure and prevents basic Deep Packet Inspection (DPI) only - e.g. pattern matching based DPI such as Snort.

The idea is to use PPTP as a nat'able channel for high performance, unencrypted GRE-tunnels. However, mppe-encrypted connections are accepted, too: It's out of scope to decide, whether basic DPI evasion is needed. If encryption ought to be secure, please consider using fastd or openvpn instead of pptp.

Unlike other exit roles, this role also creates an unbound configuration for the ppp interfaces.

Role Variables
--------------
```
accel_ppp_exit:
  ipv4:
    gateway_ip    # Address assigned to the PPtP-server when used as a gateway
    client_pool   # Address pool for PPtP tunnel addresses    
  ipv6:
    ppp_links     # Address range and allocation size for PPtP tunnel addresses
    delegate      # Address range and allocation size for IPv6 DHCPv6 subnet delegation
```

Example Playbook
-------------------

```
accel_ppp_exit
  ipv4:
    gateway_ip: 172.26.128.1
    client_pool: 172.26.129.0/24
  ipv6:
    ppp_links: fd2f:3bfe:9136::/48,64     # Use a complete subnet (ula) for ppp tunnel addresses, each tunnel gets /64
    delegate: 2001:67C:20A0:B100::/56,61  # Delegate address ranges of size /61 from 2001:67C:20A0:B100::/56
```

License
-------
GPLv3                            
