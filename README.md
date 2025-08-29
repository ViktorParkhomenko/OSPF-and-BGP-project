# OSPF-and-BGP-project
Designed and deployed a lab network using OSPF for internal routing and BGP for multi-ISP redundancy, including route redistribution and failover testing.



its going to be basic OSPFv2 configuration for routing ipv4 

🏡 Home Project: OSPFv2 Lab
🎯 Lab Goals

Configure OSPFv2 on routers R3, R4, and R5.

Place interfaces into the correct OSPF areas:

R3 → all interfaces in Area 1

R4 → Gig0/1 in Area 1, Gig0/2 in Area 0

R5 → all interfaces in Area 0

Verify reachability between loopback addresses (R1 = 1.1.1.1, R2 = 2.2.2.2, etc.).

🔧 Step 1: Review Topology (from YAML)

From the provided topology:

R3 connects to R4 on Gig0/1 (Area 1).

R4 connects to R5 on Gig0/2 (Area 0).

R1 and R2 are already preconfigured (running EIGRP/OSPF).

Each router has a Loopback0 with an IP (used as Router ID).

🔧 Step 2: Configure OSPFv2
R3 Configuration (all in Area 1)
router ospf 1
 router-id 3.3.3.3
 network 192.168.13.0 0.0.0.255 area 1
 network 192.168.34.0 0.0.0.255 area 1
 network 3.3.3.3 0.0.0.0 area 1

R4 Configuration (split between Area 0 and 1)
router ospf 1
 router-id 4.4.4.4
 network 192.168.34.0 0.0.0.255 area 1    ! G0/1
 network 192.168.45.0 0.0.0.255 area 0    ! G0/2
 network 4.4.4.4 0.0.0.0 area 0

R5 Configuration (all in Area 0)
router ospf 1
 router-id 5.5.5.5
 network 192.168.45.0 0.0.0.255 area 0
 network 192.168.25.0 0.0.0.255 area 0
 network 5.5.5.5 0.0.0.0 area 0

🔧 Step 3: Verification
Check OSPF neighbors
R3# show ip ospf neighbor
R4# show ip ospf neighbor
R5# show ip ospf neighbor


R3 ↔ R4 should be neighbors in Area 1.

R4 ↔ R5 should be neighbors in Area 0.

Check routing tables
R3# show ip route ospf
R4# show ip route ospf
R5# show ip route ospf


You should see OSPF-learned routes to other loopbacks.

End-to-End Ping
R3# ping 5.5.5.5
R5# ping 3.3.3.3
R4# ping 1.1.1.1


✅ All routers should reach each other’s loopbacks (via OSPF backbone + Area 1).

📌 Notes

R4 is acting as an ABR (Area Border Router), connecting Area 0 and Area 1.

Loopbacks are advertised as /32 host routes, so they serve as stable router IDs.

This lab demonstrates multi-area OSPF with backbone area design.
