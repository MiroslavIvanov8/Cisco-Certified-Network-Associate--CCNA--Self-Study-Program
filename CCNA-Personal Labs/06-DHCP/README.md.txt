# DHCP Labs

## Goal
Understand DHCP operation in local and routed environments.

---

## Included Labs

### DHCP_on_Server.pkt
- Basic DHCP configuration
- Local subnet leasing

Learned:
- DORA process
- UDP 67/68
- DHCP bindings

---

### DHCP_helper-address.pkt
- DHCP relay across Layer 3 boundary

Learned:
- Broadcasts do not cross routers
- Helper converts broadcast to unicast
- Server still needs return route
- DHCP conflict check behavior

---

### DHCP_Snooping.pkt
- DHCP security mechanisms

Learned:
- Trusted/untrusted ports
- Rogue DHCP protection