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

🔧 Step 1: Review Topology (from YAML)

From the provided topology:

R3 connects to R4 on Gig0/1 (Area 1).

R4 connects to R5 on Gig0/2 (Area 0).

R1 and R2 are already preconfigured (running EIGRP/OSPF).

Each router has a Loopback0 with an IP (used as Router ID).

🔧 Step 2: Configure OSPFv2
Router R1
- router ospf 1
- network 10.1.1.0 0.0.0.255 area 0
- network 1.1.1.1 0.0.0.0 area 0      # Loopback
- network 172.16.1.0 0.0.0.3 area 0
<img width="681" height="123" alt="image" src="https://github.com/user-attachments/assets/d35ded40-b643-46bc-bced-c3d01ccc563a" />

Router R2 (ABR)
- router ospf 2                       # Process ID different, still valid
- network 172.16.1.0 0.0.0.3 area 0
- network 2.2.2.2 0.0.0.0 area 1     # Loopback
- network 192.168.1.0 0.0.0.3 area 1
<img width="688" height="130" alt="image" src="https://github.com/user-attachments/assets/67779b9a-d5a9-4f37-8212-bc85059919f4" />

Router R3
- router ospf 1
- network 0.0.0.0 255.255.255.255 area 1
<img width="765" height="119" alt="image" src="https://github.com/user-attachments/assets/d6b9e543-7c23-4243-ba36-4f88aa92d305" />
We are not necessarily specifying the exact network of an interface with the network statement.
If an interface is active and its IP address falls within the defined address space, then advertise that interface’s network via OSPF.
For example, on R3, all interfaces (including the loopback) belong to the same area. Instead of writing multiple network statements, we can use a single one that covers all possible IP addresses.
Now lets confirm
- show ip route
<img width="876" height="331" alt="image" src="https://github.com/user-attachments/assets/ba796b59-55b2-49b3-874c-7384f99095f7" />
 


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
//// sh ip route 
sh ospf database 


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
