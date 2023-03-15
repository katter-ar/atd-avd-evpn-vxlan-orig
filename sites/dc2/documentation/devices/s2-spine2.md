# s2-spine2
# Table of Contents

- [Management](#management)
  - [Management Interfaces](#management-interfaces)
  - [DNS Domain](#dns-domain)
  - [NTP](#ntp)
  - [Management API HTTP](#management-api-http)
- [Authentication](#authentication)
  - [Local Users](#local-users)
- [Monitoring](#monitoring)
  - [TerminAttr Daemon](#terminattr-daemon)
- [Spanning Tree](#spanning-tree)
  - [Spanning Tree Summary](#spanning-tree-summary)
  - [Spanning Tree Device Configuration](#spanning-tree-device-configuration)
- [Internal VLAN Allocation Policy](#internal-vlan-allocation-policy)
  - [Internal VLAN Allocation Policy Summary](#internal-vlan-allocation-policy-summary)
  - [Internal VLAN Allocation Policy Configuration](#internal-vlan-allocation-policy-configuration)
- [Interfaces](#interfaces)
  - [Ethernet Interfaces](#ethernet-interfaces)
  - [Loopback Interfaces](#loopback-interfaces)
- [Routing](#routing)
  - [Service Routing Protocols Model](#service-routing-protocols-model)
  - [IP Routing](#ip-routing)
  - [IPv6 Routing](#ipv6-routing)
  - [Static Routes](#static-routes)
  - [Router BGP](#router-bgp)
- [BFD](#bfd)
  - [Router BFD](#router-bfd)
- [Multicast](#multicast)
- [Filters](#filters)
  - [Prefix-lists](#prefix-lists)
  - [Route-maps](#route-maps)
- [ACL](#acl)
- [VRF Instances](#vrf-instances)
  - [VRF Instances Summary](#vrf-instances-summary)
  - [VRF Instances Device Configuration](#vrf-instances-device-configuration)
- [Quality Of Service](#quality-of-service)

# Management

## Management Interfaces

### Management Interfaces Summary

#### IPv4

| Management Interface | description | Type | VRF | IP Address | Gateway |
| -------------------- | ----------- | ---- | --- | ---------- | ------- |
| Management0 | oob_management | oob | default | 192.168.0.21/24 | 192.168.0.1 |

#### IPv6

| Management Interface | description | Type | VRF | IPv6 Address | IPv6 Gateway |
| -------------------- | ----------- | ---- | --- | ------------ | ------------ |
| Management0 | oob_management | oob | default | - | - |

### Management Interfaces Device Configuration

```eos
!
interface Management0
   description oob_management
   no shutdown
   ip address 192.168.0.21/24
```

## DNS Domain

### DNS domain: atd.lab

### DNS Domain Device Configuration

```eos
dns domain atd.lab
!
```

## NTP

### NTP Summary

#### NTP Servers

| Server | VRF | Preferred | Burst | iBurst | Version | Min Poll | Max Poll | Local-interface | Key |
| ------ | --- | --------- | ----- | ------ | ------- | -------- | -------- | --------------- | --- |
| 192.168.0.1 | - | - | - | True | - | - | - | Management0 | - |

### NTP Device Configuration

```eos
!
ntp server 192.168.0.1 iburst local-interface Management0
```

## Management API HTTP

### Management API HTTP Summary

| HTTP | HTTPS | Default Services |
| ---- | ----- | ---------------- |
| False | True | - |

### Management API VRF Access

| VRF Name | IPv4 ACL | IPv6 ACL |
| -------- | -------- | -------- |
| default | - | - |

### Management API HTTP Configuration

```eos
!
management api http-commands
   protocol https
   no shutdown
   !
   vrf default
      no shutdown
```

# Authentication

## Local Users

### Local Users Summary

| User | Privilege | Role | Disabled |
| ---- | --------- | ---- | -------- |
| arista | 15 | network-admin | False |

### Local Users Device Configuration

```eos
!
username arista privilege 15 role network-admin secret sha512 $6$65534$Xj7Qds8fWSiFkllwXUvPx7nNc79Gc6/t1ESOFFnNgYRyWeauJew48mg2oOWkaG1w6MCLu79dSUGFa0fwkYOXW/
```

# Monitoring

## TerminAttr Daemon

### TerminAttr Daemon Summary

| CV Compression | CloudVision Servers | VRF | Authentication | Smash Excludes | Ingest Exclude | Bypass AAA |
| -------------- | ------------------- | --- | -------------- | -------------- | -------------- | ---------- |
| gzip | 192.168.0.5:9910 | - | token,/tmp/token | ale,flexCounter,hardware,kni,pulse,strata | /Sysdb/cell/1/agent,/Sysdb/cell/2/agent | False |

### TerminAttr Daemon Device Configuration

```eos
!
daemon TerminAttr
   exec /usr/bin/TerminAttr -cvaddr=192.168.0.5:9910 -cvauth=token,/tmp/token -smashexcludes=ale,flexCounter,hardware,kni,pulse,strata -ingestexclude=/Sysdb/cell/1/agent,/Sysdb/cell/2/agent -taillogs
   no shutdown
```

# Spanning Tree

## Spanning Tree Summary

STP mode: **none**

## Spanning Tree Device Configuration

```eos
!
spanning-tree mode none
```

# Internal VLAN Allocation Policy

## Internal VLAN Allocation Policy Summary

| Policy Allocation | Range Beginning | Range Ending |
| ------------------| --------------- | ------------ |
| ascending | 1006 | 1199 |

## Internal VLAN Allocation Policy Configuration

```eos
!
vlan internal order ascending range 1006 1199
```

# Interfaces

## Ethernet Interfaces

### Ethernet Interfaces Summary

#### L2

| Interface | Description | Mode | VLANs | Native VLAN | Trunk Group | Channel-Group |
| --------- | ----------- | ---- | ----- | ----------- | ----------- | ------------- |

*Inherited from Port-Channel Interface

#### IPv4

| Interface | Description | Type | Channel Group | IP Address | VRF |  MTU | Shutdown | ACL In | ACL Out |
| --------- | ----------- | -----| ------------- | ---------- | ----| ---- | -------- | ------ | ------- |
| Ethernet2 | P2P_LINK_TO_S2-LEAF1_Ethernet3 | routed | - | 10.255.1.198/31 | default | 9214 | False | - | - |
| Ethernet3 | P2P_LINK_TO_S2-LEAF2_Ethernet3 | routed | - | 10.255.1.206/31 | default | 9214 | False | - | - |
| Ethernet7 | P2P_LINK_TO_S2-BRDR1_Ethernet3 | routed | - | 10.255.1.150/31 | default | 9214 | False | - | - |
| Ethernet8 | P2P_LINK_TO_S2-BRDR2_Ethernet3 | routed | - | 10.255.1.158/31 | default | 9214 | False | - | - |

### Ethernet Interfaces Device Configuration

```eos
!
interface Ethernet2
   description P2P_LINK_TO_S2-LEAF1_Ethernet3
   no shutdown
   mtu 9214
   no switchport
   ip address 10.255.1.198/31
!
interface Ethernet3
   description P2P_LINK_TO_S2-LEAF2_Ethernet3
   no shutdown
   mtu 9214
   no switchport
   ip address 10.255.1.206/31
!
interface Ethernet7
   description P2P_LINK_TO_S2-BRDR1_Ethernet3
   no shutdown
   mtu 9214
   no switchport
   ip address 10.255.1.150/31
!
interface Ethernet8
   description P2P_LINK_TO_S2-BRDR2_Ethernet3
   no shutdown
   mtu 9214
   no switchport
   ip address 10.255.1.158/31
```

## Loopback Interfaces

### Loopback Interfaces Summary

#### IPv4

| Interface | Description | VRF | IP Address |
| --------- | ----------- | --- | ---------- |
| Loopback0 | EVPN_Overlay_Peering | default | 10.1.0.112/32 |

#### IPv6

| Interface | Description | VRF | IPv6 Address |
| --------- | ----------- | --- | ------------ |
| Loopback0 | EVPN_Overlay_Peering | default | - |


### Loopback Interfaces Device Configuration

```eos
!
interface Loopback0
   description EVPN_Overlay_Peering
   no shutdown
   ip address 10.1.0.112/32
```

# Routing
## Service Routing Protocols Model

Multi agent routing protocol model enabled

```eos
!
service routing protocols model multi-agent
```

## IP Routing

### IP Routing Summary

| VRF | Routing Enabled |
| --- | --------------- |
| default | True |
| default | false |

### IP Routing Device Configuration

```eos
!
ip routing
```
## IPv6 Routing

### IPv6 Routing Summary

| VRF | Routing Enabled |
| --- | --------------- |
| default | False |
| default | false |

## Static Routes

### Static Routes Summary

| VRF | Destination Prefix | Next Hop IP             | Exit interface      | Administrative Distance       | Tag               | Route Name                    | Metric         |
| --- | ------------------ | ----------------------- | ------------------- | ----------------------------- | ----------------- | ----------------------------- | -------------- |
| default | 0.0.0.0/0 | 192.168.0.1 | - | 1 | - | - | - |

### Static Routes Device Configuration

```eos
!
ip route 0.0.0.0/0 192.168.0.1
```

## Router BGP

### Router BGP Summary

| BGP AS | Router ID |
| ------ | --------- |
| 65100|  10.1.0.112 |

| BGP Tuning |
| ---------- |
| no bgp default ipv4-unicast |
| distance bgp 20 200 200 |
| neighbor default send-community |
| graceful-restart restart-time 300 |
| graceful-restart |
| maximum-paths 4 ecmp 4 |

### Router BGP Peer Groups

#### EVPN-OVERLAY-LOCAL-PEERS

| Settings | Value |
| -------- | ----- |
| Address Family | evpn |
| Next-hop unchanged | True |
| Source | Loopback0 |
| BFD | True |
| Ebgp multihop | 3 |
| Send community | all |
| Maximum routes | 0 (no limit) |

#### IPV4-UNDERLAY-PEERS

| Settings | Value |
| -------- | ----- |
| Address Family | ipv4 |
| Send community | all |
| Maximum routes | 12000 |

### BGP Neighbors

| Neighbor | Remote AS | VRF | Shutdown | Send-community | Maximum-routes | Allowas-in | BFD | RIB Pre-Policy Retain | Route-Reflector Client |
| -------- | --------- | --- | -------- | -------------- | -------------- | ---------- | --- | --------------------- | ---------------------- |
| 10.1.0.102 | 65199 | default | - | Inherited from peer group EVPN-OVERLAY-LOCAL-PEERS | Inherited from peer group EVPN-OVERLAY-LOCAL-PEERS | - | Inherited from peer group EVPN-OVERLAY-LOCAL-PEERS | - | - |
| 10.1.0.104 | 65199 | default | - | Inherited from peer group EVPN-OVERLAY-LOCAL-PEERS | Inherited from peer group EVPN-OVERLAY-LOCAL-PEERS | - | Inherited from peer group EVPN-OVERLAY-LOCAL-PEERS | - | - |
| 10.1.0.114 | 65101 | default | - | Inherited from peer group EVPN-OVERLAY-LOCAL-PEERS | Inherited from peer group EVPN-OVERLAY-LOCAL-PEERS | - | Inherited from peer group EVPN-OVERLAY-LOCAL-PEERS | - | - |
| 10.1.0.116 | 65101 | default | - | Inherited from peer group EVPN-OVERLAY-LOCAL-PEERS | Inherited from peer group EVPN-OVERLAY-LOCAL-PEERS | - | Inherited from peer group EVPN-OVERLAY-LOCAL-PEERS | - | - |
| 10.255.1.151 | 65199 | default | - | Inherited from peer group IPV4-UNDERLAY-PEERS | Inherited from peer group IPV4-UNDERLAY-PEERS | - | - | - | - |
| 10.255.1.159 | 65199 | default | - | Inherited from peer group IPV4-UNDERLAY-PEERS | Inherited from peer group IPV4-UNDERLAY-PEERS | - | - | - | - |
| 10.255.1.199 | 65101 | default | - | Inherited from peer group IPV4-UNDERLAY-PEERS | Inherited from peer group IPV4-UNDERLAY-PEERS | - | - | - | - |
| 10.255.1.207 | 65101 | default | - | Inherited from peer group IPV4-UNDERLAY-PEERS | Inherited from peer group IPV4-UNDERLAY-PEERS | - | - | - | - |

### Router BGP EVPN Address Family

#### EVPN Peer Groups

| Peer Group | Activate |
| ---------- | -------- |
| EVPN-OVERLAY-LOCAL-PEERS | True |

### Router BGP Device Configuration

```eos
!
router bgp 65100
   router-id 10.1.0.112
   no bgp default ipv4-unicast
   distance bgp 20 200 200
   neighbor default send-community
   graceful-restart restart-time 300
   graceful-restart
   maximum-paths 4 ecmp 4
   neighbor EVPN-OVERLAY-LOCAL-PEERS peer group
   neighbor EVPN-OVERLAY-LOCAL-PEERS next-hop-unchanged
   neighbor EVPN-OVERLAY-LOCAL-PEERS update-source Loopback0
   neighbor EVPN-OVERLAY-LOCAL-PEERS bfd
   neighbor EVPN-OVERLAY-LOCAL-PEERS ebgp-multihop 3
   neighbor EVPN-OVERLAY-LOCAL-PEERS password 7 HwsUtvRBaFIOmUOBhrtIfQ==
   neighbor EVPN-OVERLAY-LOCAL-PEERS send-community
   neighbor EVPN-OVERLAY-LOCAL-PEERS maximum-routes 0
   neighbor IPV4-UNDERLAY-PEERS peer group
   neighbor IPV4-UNDERLAY-PEERS password 7 X/OrLne2MxtUwJOXyoEcEw==
   neighbor IPV4-UNDERLAY-PEERS send-community
   neighbor IPV4-UNDERLAY-PEERS maximum-routes 12000
   neighbor 10.1.0.102 peer group EVPN-OVERLAY-LOCAL-PEERS
   neighbor 10.1.0.102 remote-as 65199
   neighbor 10.1.0.102 description s2-brdr1
   neighbor 10.1.0.104 peer group EVPN-OVERLAY-LOCAL-PEERS
   neighbor 10.1.0.104 remote-as 65199
   neighbor 10.1.0.104 description s2-brdr2
   neighbor 10.1.0.114 peer group EVPN-OVERLAY-LOCAL-PEERS
   neighbor 10.1.0.114 remote-as 65101
   neighbor 10.1.0.114 description s2-leaf1
   neighbor 10.1.0.116 peer group EVPN-OVERLAY-LOCAL-PEERS
   neighbor 10.1.0.116 remote-as 65101
   neighbor 10.1.0.116 description s2-leaf2
   neighbor 10.255.1.151 peer group IPV4-UNDERLAY-PEERS
   neighbor 10.255.1.151 remote-as 65199
   neighbor 10.255.1.151 description s2-brdr1_Ethernet3
   neighbor 10.255.1.159 peer group IPV4-UNDERLAY-PEERS
   neighbor 10.255.1.159 remote-as 65199
   neighbor 10.255.1.159 description s2-brdr2_Ethernet3
   neighbor 10.255.1.199 peer group IPV4-UNDERLAY-PEERS
   neighbor 10.255.1.199 remote-as 65101
   neighbor 10.255.1.199 description s2-leaf1_Ethernet3
   neighbor 10.255.1.207 peer group IPV4-UNDERLAY-PEERS
   neighbor 10.255.1.207 remote-as 65101
   neighbor 10.255.1.207 description s2-leaf2_Ethernet3
   redistribute connected route-map RM-CONN-2-BGP
   !
   address-family evpn
      neighbor EVPN-OVERLAY-LOCAL-PEERS activate
   !
   address-family ipv4
      no neighbor EVPN-OVERLAY-LOCAL-PEERS activate
      neighbor IPV4-UNDERLAY-PEERS activate
```

# BFD

## Router BFD

### Router BFD Multihop Summary

| Interval | Minimum RX | Multiplier |
| -------- | ---------- | ---------- |
| 300 | 300 | 3 |

### Router BFD Device Configuration

```eos
!
router bfd
   multihop interval 300 min-rx 300 multiplier 3
```

# Multicast

# Filters

## Prefix-lists

### Prefix-lists Summary

#### PL-LOOPBACKS-EVPN-OVERLAY

| Sequence | Action |
| -------- | ------ |
| 10 | permit 10.1.0.0/24 eq 32 |

### Prefix-lists Device Configuration

```eos
!
ip prefix-list PL-LOOPBACKS-EVPN-OVERLAY
   seq 10 permit 10.1.0.0/24 eq 32
```

## Route-maps

### Route-maps Summary

#### RM-CONN-2-BGP

| Sequence | Type | Match | Set | Sub-Route-Map | Continue |
| -------- | ---- | ----- | --- | ------------- | -------- |
| 10 | permit | ip address prefix-list PL-LOOPBACKS-EVPN-OVERLAY | - | - | - |

### Route-maps Device Configuration

```eos
!
route-map RM-CONN-2-BGP permit 10
   match ip address prefix-list PL-LOOPBACKS-EVPN-OVERLAY
```

# ACL

# VRF Instances

## VRF Instances Summary

| VRF Name | IP Routing |
| -------- | ---------- |
| default | disabled |

## VRF Instances Device Configuration

```eos
```

# Quality Of Service