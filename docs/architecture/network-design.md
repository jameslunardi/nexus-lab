# Network Architecture Design

## Overview

The Nexus Lab network architecture consists of two separate subnets to simulate a multi-domain enterprise environment. Both networks are managed by libvirt with routing enabled between them. All VMs use static IP addresses.

---

## Network Topology

```
┌─────────────────────────────────────────────────────────────┐
│                    Physical Host (Arch Linux)                │
│                                                              │
│  ┌────────────────────────────────────────────────────────┐ │
│  │              libvirt Virtual Networks                   │ │
│  │                                                          │ │
│  │  ┌──────────────────────┐  ┌──────────────────────┐   │ │
│  │  │   Domain 1 Network   │  │   Domain 2 Network   │   │ │
│  │  │   10.0.100.0/24      │  │   10.0.101.0/24      │   │ │
│  │  │                      │  │                      │   │ │
│  │  │  Gateway: .1         │  │  Gateway: .1         │   │ │
│  │  │                      │  │                      │   │ │
│  │  │  ┌────────────────┐  │  │  ┌────────────────┐  │   │ │
│  │  │  │     DC01       │  │  │  │     DC02       │  │   │ │
│  │  │  │  10.0.100.10   │  │  │  │  10.0.101.10   │  │   │ │
│  │  │  │  (Domain 1 DC) │  │  │  │  (Domain 2 DC) │  │   │ │
│  │  │  └────────────────┘  │  │  └────────────────┘  │   │ │
│  │  │                      │  │                      │   │ │
│  │  │                      │  │  ┌────────────────┐  │   │ │
│  │  │                      │  │  │   ADSync01     │  │   │ │
│  │  │                      │  │  │  10.0.101.20   │  │   │ │
│  │  │                      │  │  │  (Member Srv)  │  │   │ │
│  │  │                      │  │  └────────────────┘  │   │ │
│  │  │                      │  │                      │   │ │
│  │  └──────────────────────┘  └──────────────────────┘   │ │
│  │                                                          │ │
│  │              Libvirt Routing Enabled                    │ │
│  └────────────────────────────────────────────────────────┘ │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

---

## Network Specifications

### Domain 1 Network

**Network Details:**
- **Subnet**: 10.0.100.0/24
- **Netmask**: 255.255.255.0
- **Gateway**: 10.0.100.1 (libvirt)
- **Broadcast**: 10.0.100.255
- **Usable IPs**: 10.0.100.1 - 10.0.100.254

**IP Allocation:**
- **10.0.100.1** - Gateway (libvirt)
- **10.0.100.10** - DC01 (Domain Controller 1) - Static
- **10.0.100.11 - 10.0.100.254** - Reserved for future static assignments

**Services:**
- **DHCP**: Disabled (all VMs use static IPs)
- **DNS**: Provided by DC01 (10.0.100.10)
- **Routing**: Enabled to Domain 2 network

### Domain 2 Network

**Network Details:**
- **Subnet**: 10.0.101.0/24
- **Netmask**: 255.255.255.0
- **Gateway**: 10.0.101.1 (libvirt)
- **Broadcast**: 10.0.101.255
- **Usable IPs**: 10.0.101.1 - 10.0.101.254

**IP Allocation:**
- **10.0.101.1** - Gateway (libvirt)
- **10.0.101.10** - DC02 (Domain Controller 2) - Static
- **10.0.101.20** - ADSync01 (Member Server) - Static
- **10.0.101.21 - 10.0.101.254** - Reserved for future static assignments

**Services:**
- **DHCP**: Disabled (all VMs use static IPs)
- **DNS**: Provided by DC02 (10.0.101.10)
- **Routing**: Enabled to Domain 1 network

---

## Active Directory Configuration

### Domain 1

**Domain Details:**
- **Domain Name**: corp.lab
- **NetBIOS Name**: CORP
- **Forest Functional Level**: Windows Server 2025
- **Domain Controller**: DC01 (10.0.100.10)
- **DNS Server**: DC01 (10.0.100.10)
- **Member Servers**: None (ADSync01 is in Domain 2)

### Domain 2

**Domain Details:**
- **Domain Name**: prod.lab
- **NetBIOS Name**: PROD
- **Forest Functional Level**: Windows Server 2025
- **Domain Controller**: DC02 (10.0.101.10)
- **DNS Server**: DC02 (10.0.101.10)
- **Member Servers**: ADSync01 (10.0.101.20)

**Domain Relationship:**
- Separate forests (no trust initially)
- Cross-domain connectivity via routing
- ADSync tool will synchronise users from corp.lab to prod.lab

---

## Routing Configuration

### Inter-Network Routing

**Routing Method**: Libvirt built-in routing

**Routing Rules:**
- Domain 1 → Domain 2: Routes through libvirt (10.0.100.1 → 10.0.101.0/24)
- Domain 2 → Domain 1: Routes through libvirt (10.0.101.1 → 10.0.100.0/24)

**Connectivity:**
- DC01 can reach DC02 directly (10.0.100.10 → 10.0.101.10)
- DC02 can reach DC01 directly (10.0.101.10 → 10.0.100.10)
- ADSync01 can reach both DC01 and DC02

### DNS Configuration

**DNS Resolution Strategy:**

**Domain 1 VMs:**
- Primary DNS: 10.0.100.10 (DC01)
- Secondary DNS: 10.0.101.10 (DC02) - for cross-domain resolution

**Domain 2 VMs:**
- Primary DNS: 10.0.101.10 (DC02)
- Secondary DNS: 10.0.100.10 (DC01) - for cross-domain resolution

**DNS Forwarders:**
- Both DCs will use external DNS forwarders (e.g., 8.8.8.8, 1.1.1.1) for internet resolution

---

## Security Considerations

### Network Segmentation

**Isolation:**
- Two separate layer 2 broadcast domains
- Routing controlled by libvirt
- Future firewall rules can be added if needed

**Access Control:**
- No firewall rules initially (full connectivity between networks)
- Future enhancement: Add iptables/firewall rules for specific service access

### DNS Security

- DNS zones are separate (no zone transfers)
- Conditional forwarders can be configured if needed
- DNSSEC not implemented (overkill for lab environment)

---

## Terraform Implementation Strategy

### Module Design

The network configuration will be implemented as a reusable Terraform module:

**Module Location**: `terraform/modules/network/`

**Module Inputs:**
- Network name
- Subnet CIDR
- Gateway IP
- DNS domain name (optional)
- DHCP enabled (boolean, default: false)

**Module Outputs:**
- Network ID
- Network name
- Gateway IP
- Subnet details

### Network Creation Order

1. **Domain 1 Network** - Created first (primary network)
2. **Domain 2 Network** - Created second (secondary network)
3. **Routing** - Configured automatically by libvirt when both networks exist

---

## Testing Strategy

### Network Validation

**Basic Connectivity Tests:**
1. Verify both networks are created in libvirt
2. Verify routing between networks
3. Test DNS resolution within each network (after DCs are configured)

**Cross-Network Tests:**
1. Ping from Domain 1 to Domain 2
2. Ping from Domain 2 to Domain 1
3. DNS queries across networks
4. Trace route between networks

**Tools:**
- `virsh net-list` - List networks
- `virsh net-info` - Network details
- `virsh net-dumpxml` - Full network configuration
- Guest OS tools (ping, tracert, nslookup)

---

## Future Enhancements

### Potential Improvements

**Phase 2+:**
- Add DHCP if additional VMs are deployed
- Add firewall rules between networks
- Implement network monitoring
- Add additional subnets for DMZ or management
- Configure VLANs if needed
- Add site-to-site VPN (if cloud integration added)

**Advanced DNS:**
- Configure conditional forwarders between domains
- Implement DNS zone transfers
- Add reverse DNS zones

---

## Design Decisions

### Why Separate Subnets?

**Rationale:**
- More realistic enterprise scenario
- Demonstrates multi-network management
- Shows routing capabilities
- Better simulates real AD sync scenarios
- Allows future network segmentation features

### Why Libvirt Routing vs Router VM?

**Rationale:**
- Simpler initial setup
- Fewer resources required
- Easier to troubleshoot
- Built-in functionality
- Can migrate to router VM in future if needed

### Why /24 Subnets?

**Rationale:**
- Plenty of IP addresses for growth
- Simple to calculate and understand
- Standard enterprise sizing
- Easy to manage
- Room for future expansion

### Why ADSync01 in Domain 2?

**Rationale:**
- Server runs in target domain (prod.lab)
- Pulls data from source domain (corp.lab)
- More typical enterprise pattern
- Better demonstrates cross-domain authentication
- Simpler credential management in production environment

### Why No DHCP Initially?

**Rationale:**
- Only three VMs, all infrastructure servers
- Static IPs are standard for servers
- Simpler configuration and troubleshooting
- No DHCP dependencies during testing
- Can be added later if needed for additional VMs

---

## Network Diagram Legend

```
┌─────────┐
│  Box    │  = Network or VM
└─────────┘

─────────   = Connection/Link

.1, .10     = IP address suffix (last octet)
```

---

## References

### Standards and Best Practices

- RFC 1918 - Private IP Address Space
- RFC 2136 - Dynamic Updates in DNS
- Microsoft Active Directory DNS Best Practices
- Libvirt Network Documentation: https://libvirt.org/formatnetwork.html

### Related Documentation

- `/docs/setup/` - Setup and installation guides
- `/terraform/modules/network/` - Network module implementation
- `/docs/runbooks/` - Operational procedures

---

**Document Version:** 1.2  
**Last Updated:** 17 October 2025  
**Next Review:** On completion of network module implementation  
**Status:** Approved - Ready for Implementation
