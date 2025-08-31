# OSPFv4 configuration for routing IPv4
In this lab, OSPFv2 was configured with R3 in Area 1, R5 in Area 0, and R4 as the ABR connecting both areas. Neighbor adjacencies formed correctly, routes were exchanged, and end-to-end connectivity between all loopbacks was verified.
<img width="949" height="322" alt="image" src="https://github.com/user-attachments/assets/86035f4b-62a9-4534-bb4a-6e03a37abdd4" />


Configure OSPFv2 on routers R3, R4, and R5.

Place interfaces into the correct OSPF areas:

R3 → all interfaces in Area 1

R4 → Gig0/1 in Area 1, Gig0/2 in Area 0

R5 → all interfaces in Area 0

Verify reachability between loopback addresses (R1 = 1.1.1.1, R2 = 2.2.2.2, etc.).

<img width="874" height="127" alt="image" src="https://github.com/user-attachments/assets/6139ef8a-a5ff-45db-9e18-1a81eb0d1c5c" />
<img width="865" height="130" alt="image" src="https://github.com/user-attachments/assets/a91264a0-bd45-4c8d-a47a-60930bf90714" />

### Step 1: Review Topology (from YAML)

From the provided topology:

R3 connects to R4 on Gig0/1 (Area 1).

R4 connects to R5 on Gig0/2 (Area 0).

R1 and R2 are already preconfigured (running EIGRP/OSPF).

Each router has a Loopback0 with an IP (used as Router ID).

### Step 2: Configure OSPFv2
Router R3

All interfaces should participate in Area 1.

- router ospf 1
- network 0.0.0.0 255.255.255.255 area 1
<img width="787" height="236" alt="image" src="https://github.com/user-attachments/assets/783f9db9-779d-4c05-a0ec-bba3238db63b" />

Router R4

Some interfaces belong to Area 1, others to Area 0.

- router ospf 1
- network 4.4.4.4 0.0.0.0 area 1        # Loopback
- network 198.51.100.0 0.0.0.3 area 1     # /30 link
- network 203.0.113.0 0.0.0.3 area 0    # /30 link
<img width="784" height="202" alt="image" src="https://github.com/user-attachments/assets/10d68376-53a2-4d87-b730-c823ea417f7a" />


✅ R4 participates in both Area 0 and Area 1 (acts as an ABR).

Router R5

All interfaces belong to Area 0.

router ospf 1
 network 0.0.0.0 255.255.255.255 area 0
<img width="763" height="163" alt="image" src="https://github.com/user-attachments/assets/82cb96f4-c430-4735-8394-b29a2cd58b59" />


✅ After a short time, adjacencies form with neighbors.

Verification
On Router R5
show ip route
<img width="892" height="318" alt="image" src="https://github.com/user-attachments/assets/5b00e999-13aa-47e9-81c7-e21898a17d8a" />


Learned all loopbacks (1.1.1.1, 2.2.2.2, 3.3.3.3, 4.4.4.4).

OSPF inter-area routes are marked with IA.

All subnets are present, confirming full routing knowledge.

show ip ospf database


Output shows entries for Area 0 only (since R5 only participates in the backbone).

On Router R4
- show ip ospf database
<img width="934" height="647" alt="image" src="https://github.com/user-attachments/assets/13763eda-b4cc-4ecc-ab58-ec3e705399b5" />


Output contains information for both Area 0 and Area 1.

Confirms R4 is functioning correctly as an ABR.

Conclusion

OSPFv2 successfully configured on R3, R4, and R5.

Adjacencies formed as expected across backbone and non-backbone areas.

Verified routing tables and OSPF databases confirm that all routers have complete topology awareness.







This lab demonstrates multi-area OSPF with backbone area design.
