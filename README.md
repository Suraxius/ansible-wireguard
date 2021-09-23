# WireGuard VPN Mesh

This ansible role manages the configuration of one or more wireguard mesh VPNs

## Support
- Ubuntu
- Debian
- Raspberry PI OS

## Sample configuration

```
wg:
- name: wg0
  subnet: 10.0.0.0/16
  hosts:
  - id:      1
    name:    inventory_hostname
    address: ip/domain this host ist reachable on
    port:    udp port to listen for incoming connections on
    privkey: XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
    pubkey:  YYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYY
    postup:
    - iptables -A INPUT -i %i -m state --state ESTABLISHED,RELATED -j ACCEPT
    - iptables -A INPUT -i %i -p icmp --icmp-type 8 -j ACCEPT
    - iptables -A INPUT -i %i -j DROP
    postdown:
    - iptables -D INPUT -i %i -m state --state ESTABLISHED,RELATED -j ACCEPT
    - iptables -D INPUT -i %i -p icmp --icmp-type 8 -j ACCEPT
    - iptables -D INPUT -i %i -j DROP

  - id:      2
    name:    inventory_hostname
    privkey: XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
    pubkey:  YYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYY

  - id:      3
    name:    inventory_hostname
    privkey: XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
    pubkey:  YYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYY

  - id:      4
    name:    inventory_hostname
    privkey: XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
    pubkey:  YYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYY

  - id:      5
    name:    inventory_hostname
    privkey: XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
    pubkey:  YYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYY
```

| :exclamation:  Make sure the hostnames configured (wg.hosts.name) match the inventory hostnames. |
|-----------------------------------------|


# Peer Linking
Note that each host with the 'address' property will be configured on every host as a peer. Without it a host will be considered unreachable from without and will be configured to initiate connections to other hosts with the property defined.

