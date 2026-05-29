# ACL-NAT/PAT-DHCP Lab

## Goal

Build a small enterprise network using VLANs, DHCP, ACLs and PAT. Validate packet flow and troubleshoot common Layer 2, Layer 3 and IP Services issues.

---

## Topology

### VLANs

| VLAN | Purpose | Network         |
| ---- | ------- | --------------- |
| 10   | Staff   | 192.168.10.0/24 |
| 20   | Guest   | 192.168.20.0/24 |
| 30   | Servers | 192.168.30.0/24 |

### WAN

R1 Serial2/0

200.1.1.2/30

ISP

200.1.1.1/30

---

## Features Implemented

### Router-on-a-Stick

Configured subinterfaces:

* Fa0/0.10
* Fa0/0.20
* Fa0/0.30

with IEEE 802.1Q encapsulation.

---

### DHCP

Configured DHCP pools for:

* VLAN10
* VLAN20

Configured excluded addresses.

Verified DORA process.

---

### ACLs

#### Guest VLAN Isolation

Guests were denied access to private internal networks while still being allowed Internet access.

Implemented using extended ACL applied inbound on VLAN20 subinterface.

#### Server Protection

Only Staff VLAN permitted Telnet access to the server.

All other access to the server denied.

---

### PAT (NAT Overload)

Configured PAT using:

* NAT ACL
* Inside interfaces
* Outside interface
* Overload statement

All internal clients share the public Serial2/0 address.

---

## Troubleshooting Performed

### Issue 1

PAT translations were not occurring.

Cause:

NAT ACL did not correctly match private networks.

Resolution:

Corrected wildcard mask and ACL entries.

---

### Issue 2

PAT translations still failed.

Cause:

ip nat inside was configured on parent interface instead of router-on-a-stick subinterfaces.

Resolution:

Configured ip nat inside on:

* Fa0/0.10
* Fa0/0.20
* Fa0/0.30

---

### Issue 3

Internet traffic dropped after NAT.

Cause:

Outbound ACL contained only:

deny ip 192.168.0.0 0.0.255.255 any

Traffic later matched the implicit deny.

Resolution:

Added explicit permit statement.

---

### Issue 4

Server Telnet test failed.

Cause:

ACL permitted traffic correctly but Telnet service was not listening.

Resolution:

Verified application-layer service behavior.

---

## Key Lessons Learned

### ACL Processing

Every ACL ends with:

deny ip any any

even when not visible.

---

### PAT Requirements

PAT requires:

* Matching ACL
* ip nat inside
* ip nat outside
* overload configuration

Missing any component causes translation failure.

---

### Router-on-a-Stick

Traffic enters subinterfaces.

Services such as NAT and ACLs should often be applied to subinterfaces rather than the parent interface.

---

### Packet Flow Order

A useful troubleshooting sequence:

1. Layer 2
2. Routing
3. NAT
4. ACL
5. Application/Service

Following packet flow made troubleshooting significantly easier.

---

## Useful Verification Commands

show ip route

show access-lists

show ip nat translations

show ip nat statistics

show ip interface brief

show running-config

show interfaces trunk

show vlan brief

---

## Overall Result

Successfully implemented and validated:

* VLAN segmentation
* Inter-VLAN routing
* DHCP services
* Guest isolation
* Server protection
* PAT Internet access

while troubleshooting multiple intentional and unintentional configuration errors.

### ACL and NAT Interaction

Troubleshooting demonstrated that packet processing depends on ACL placement.

Ingress ACLs filter traffic before routing decisions are completed.

Egress ACLs filter traffic after routing and NAT processing.

Packet Tracer simulation mode was used to verify packet traversal through:

- Ingress ACLs
- Routing table lookup
- NAT translation
- Egress ACLs
- Final forwarding