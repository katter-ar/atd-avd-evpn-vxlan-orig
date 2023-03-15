# dc2_fabric

# Table of Contents

- [Fabric Switches and Management IP](#fabric-switches-and-management-ip)
  - [Fabric Switches with inband Management IP](#fabric-switches-with-inband-management-ip)
- [Fabric Topology](#fabric-topology)
- [Fabric IP Allocation](#fabric-ip-allocation)
  - [Fabric Point-To-Point Links](#fabric-point-to-point-links)
  - [Point-To-Point Links Node Allocation](#point-to-point-links-node-allocation)
  - [Loopback Interfaces (BGP EVPN Peering)](#loopback-interfaces-bgp-evpn-peering)
  - [Loopback0 Interfaces Node Allocation](#loopback0-interfaces-node-allocation)
  - [VTEP Loopback VXLAN Tunnel Source Interfaces (VTEPs Only)](#vtep-loopback-vxlan-tunnel-source-interfaces-vteps-only)
  - [VTEP Loopback Node allocation](#vtep-loopback-node-allocation)

# Fabric Switches and Management IP

| POD | Type | Node | Management IP | Platform | Provisioned in CloudVision |
| --- | ---- | ---- | ------------- | -------- | -------------------------- |
| dc2_fabric | l3leaf | s2-brdr1 | 192.168.0.200/24 | ceos | Provisioned |
| dc2_fabric | l3leaf | s2-brdr2 | 192.168.0.201/24 | ceos | Provisioned |
| dc2_fabric | l3leaf | s2-leaf1 | 192.168.0.22/24 | ceos | Provisioned |
| dc2_fabric | l3leaf | s2-leaf2 | 192.168.0.23/24 | ceos | Provisioned |
| dc2_fabric | l3leaf | s2-leaf3 | 192.168.0.24/24 | ceos | Provisioned |
| dc2_fabric | l3leaf | s2-leaf4 | 192.168.0.25/24 | ceos | Provisioned |
| dc2_fabric | spine | s2-spine1 | 192.168.0.20/24 | ceos | Provisioned |
| dc2_fabric | spine | s2-spine2 | 192.168.0.21/24 | ceos | Provisioned |

> Provision status is based on Ansible inventory declaration and do not represent real status from CloudVision.

## Fabric Switches with inband Management IP
| POD | Type | Node | Management IP | Inband Interface |
| --- | ---- | ---- | ------------- | ---------------- |

# Fabric Topology

| Type | Node | Node Interface | Peer Type | Peer Node | Peer Interface |
| ---- | ---- | -------------- | --------- | ----------| -------------- |
| l3leaf | s2-brdr1 | Ethernet1 | mlag_peer | s2-brdr2 | Ethernet1 |
| l3leaf | s2-brdr1 | Ethernet2 | spine | s2-spine1 | Ethernet7 |
| l3leaf | s2-brdr1 | Ethernet3 | spine | s2-spine2 | Ethernet7 |
| l3leaf | s2-brdr1 | Ethernet6 | mlag_peer | s2-brdr2 | Ethernet6 |
| l3leaf | s2-brdr2 | Ethernet2 | spine | s2-spine1 | Ethernet8 |
| l3leaf | s2-brdr2 | Ethernet3 | spine | s2-spine2 | Ethernet8 |
| l3leaf | s2-leaf1 | Ethernet1 | mlag_peer | s2-leaf2 | Ethernet1 |
| l3leaf | s2-leaf1 | Ethernet2 | spine | s2-spine1 | Ethernet2 |
| l3leaf | s2-leaf1 | Ethernet3 | spine | s2-spine2 | Ethernet2 |
| l3leaf | s2-leaf1 | Ethernet6 | mlag_peer | s2-leaf2 | Ethernet6 |
| l3leaf | s2-leaf2 | Ethernet2 | spine | s2-spine1 | Ethernet3 |
| l3leaf | s2-leaf2 | Ethernet3 | spine | s2-spine2 | Ethernet3 |
| l3leaf | s2-leaf3 | Ethernet1 | mlag_peer | s2-leaf4 | Ethernet1 |
| l3leaf | s2-leaf3 | Ethernet2 | spine | s2-spine1 | Ethernet4 |
| l3leaf | s2-leaf3 | Ethernet3 | spine | s2-spine2 | Ethernet4 |
| l3leaf | s2-leaf3 | Ethernet6 | mlag_peer | s2-leaf4 | Ethernet6 |
| l3leaf | s2-leaf4 | Ethernet2 | spine | s2-spine1 | Ethernet5 |
| l3leaf | s2-leaf4 | Ethernet3 | spine | s2-spine2 | Ethernet5 |

# Fabric IP Allocation

## Fabric Point-To-Point Links

| Uplink IPv4 Pool | Available Addresses | Assigned addresses | Assigned Address % |
| ---------------- | ------------------- | ------------------ | ------------------ |
| 10.255.0.0/22 | 1024 | 24 | 2.35 % |

## Point-To-Point Links Node Allocation

| Node | Node Interface | Node IP Address | Peer Node | Peer Interface | Peer IP Address |
| ---- | -------------- | --------------- | --------- | -------------- | --------------- |
| s2-brdr1 | Ethernet2 | 10.255.1.149/31 | s2-spine1 | Ethernet7 | 10.255.1.148/31 |
| s2-brdr1 | Ethernet3 | 10.255.1.151/31 | s2-spine2 | Ethernet7 | 10.255.1.150/31 |
| s2-brdr2 | Ethernet2 | 10.255.1.157/31 | s2-spine1 | Ethernet8 | 10.255.1.156/31 |
| s2-brdr2 | Ethernet3 | 10.255.1.159/31 | s2-spine2 | Ethernet8 | 10.255.1.158/31 |
| s2-leaf1 | Ethernet2 | 10.255.1.197/31 | s2-spine1 | Ethernet2 | 10.255.1.196/31 |
| s2-leaf1 | Ethernet3 | 10.255.1.199/31 | s2-spine2 | Ethernet2 | 10.255.1.198/31 |
| s2-leaf2 | Ethernet2 | 10.255.1.205/31 | s2-spine1 | Ethernet3 | 10.255.1.204/31 |
| s2-leaf2 | Ethernet3 | 10.255.1.207/31 | s2-spine2 | Ethernet3 | 10.255.1.206/31 |
| s2-leaf3 | Ethernet2 | 10.255.1.213/31 | s2-spine1 | Ethernet4 | 10.255.1.212/31 |
| s2-leaf3 | Ethernet3 | 10.255.1.215/31 | s2-spine2 | Ethernet4 | 10.255.1.214/31 |
| s2-leaf4 | Ethernet2 | 10.255.1.221/31 | s2-spine1 | Ethernet5 | 10.255.1.220/31 |
| s2-leaf4 | Ethernet3 | 10.255.1.223/31 | s2-spine2 | Ethernet5 | 10.255.1.222/31 |

## Loopback Interfaces (BGP EVPN Peering)

| Loopback Pool | Available Addresses | Assigned addresses | Assigned Address % |
| ------------- | ------------------- | ------------------ | ------------------ |
| 10.1.0.0/24 | 256 | 8 | 3.13 % |

## Loopback0 Interfaces Node Allocation

| POD | Node | Loopback0 |
| --- | ---- | --------- |
| dc2_fabric | s2-brdr1 | 10.1.0.102/32 |
| dc2_fabric | s2-brdr2 | 10.1.0.104/32 |
| dc2_fabric | s2-leaf1 | 10.1.0.114/32 |
| dc2_fabric | s2-leaf2 | 10.1.0.116/32 |
| dc2_fabric | s2-leaf3 | 10.1.0.118/32 |
| dc2_fabric | s2-leaf4 | 10.1.0.120/32 |
| dc2_fabric | s2-spine1 | 10.1.0.110/32 |
| dc2_fabric | s2-spine2 | 10.1.0.112/32 |

## VTEP Loopback VXLAN Tunnel Source Interfaces (VTEPs Only)

| VTEP Loopback Pool | Available Addresses | Assigned addresses | Assigned Address % |
| --------------------- | ------------------- | ------------------ | ------------------ |
| 10.1.1.0/24 | 256 | 6 | 2.35 % |

## VTEP Loopback Node allocation

| POD | Node | Loopback1 |
| --- | ---- | --------- |
| dc2_fabric | s2-brdr1 | 10.1.1.102/32 |
| dc2_fabric | s2-brdr2 | 10.1.1.102/32 |
| dc2_fabric | s2-leaf1 | 10.1.1.114/32 |
| dc2_fabric | s2-leaf2 | 10.1.1.114/32 |
| dc2_fabric | s2-leaf3 | 10.1.1.118/32 |
| dc2_fabric | s2-leaf4 | 10.1.1.118/32 |
