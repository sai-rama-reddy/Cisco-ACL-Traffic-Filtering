# Cisco ACL Traffic Filtering Project Using GNS3

## Project Overview
This project demonstrates the implementation of Access Control Lists (ACLs) using Cisco routers and switches in GNS3. The objective is to control network traffic by allowing or denying specific hosts access to a destination router based on their IP addresses.

The lab uses an extended ACL to restrict PC2 from accessing R2 while allowing PC1, PC3, and PC4 to communicate with R2.

## Objectives
- Configure router interfaces and IP addressing.
- Implement extended ACLs to filter traffic based on source and destination IP addresses.
- Allow selected PCs to access R2 while blocking unauthorized traffic from PC2.
- Verify connectivity and troubleshoot ACL-related issues.
- Understand ACL placement and packet-filtering behavior.

## Network Topology

**Devices used:**
- Routers: R1 and R2
- Switches: Switch1, Switch2, Switch3
- End devices: PC1, PC2, PC3, PC4
- Network simulation tool: GNS3

### IP Addressing Table

| Device | IP Address | Network |
|---|---|---|
| PC1 | 192.168.0.10 | 192.168.0.0/24 |
| PC3 | 192.168.0.11 | 192.168.0.0/24 |
| R1 G1/0 | 192.168.0.1 | 192.168.0.0/24 |
| PC2 | 172.16.0.10 | 172.16.0.0/24 |
| PC4 | 172.16.0.11 | 172.16.0.0/24 |
| R1 G2/0 | 172.16.0.1 | 172.16.0.0/24 |
| R1 F0/0 | 10.0.0.1 | R1–R2 link |
| R2 loopback/destination | 8.8.8.8 | As configured |

**Note:** Configure the R2 interface and routing according to your actual lab. The example ACL below assumes `8.8.8.8` is the destination used to test access to R2.

## ACL Requirements

| Source | Destination | Expected result |
|---|---|---|
| PC1 – 192.168.0.10 | R2 – 8.8.8.8 | Permit |
| PC2 – 172.16.0.10 | R2 – 8.8.8.8 | Deny |
| PC3 – 192.168.0.11 | R2 – 8.8.8.8 | Permit |
| PC4 – 172.16.0.11 | R2 – 8.8.8.8 | Permit |

## Configuration

### 1. Configure PC IP Addresses

On each VPCS terminal, configure the IP address and default gateway.

**PC1**
```text
ip 192.168.0.10/24 192.168.0.1
```

**PC2**
```text
ip 172.16.0.10/24 172.16.0.1
```

**PC3**
```text
ip 192.168.0.11/24 192.168.0.1
```

**PC4**
```text
ip 172.16.0.11/24 172.16.0.1
```

### 2. Configure the Extended ACL on R1

```cisco
enable
configure terminal

ip access-list extended BLOCK_PC2_TO_R2
 deny ip host 172.16.0.10 host 8.8.8.8
 permit ip any any
exit
```

This ACL denies traffic from PC2 to `8.8.8.8` while permitting other IP traffic that matches the remaining rules.

### 3. Apply the ACL to the Correct Interface

Apply the ACL inbound on R1's interface connected to Switch2.

```cisco
interface GigabitEthernet2/0
 ip access-group BLOCK_PC2_TO_R2 in
exit

end
write memory
```

**Important:** If you also need to block PC2 from accessing R2's transit interface, add a deny statement for PC2 to R2's actual transit IP before `permit ip any any`. Replace the interface names if your device uses different names.

### 4. Verify the ACL

```cisco
show access-lists
show ip interface GigabitEthernet2/0
show ip route
show running-config
```

Check that the ACL is applied to the correct interface and that its match counters increase when you test traffic.

## Testing and Expected Results

From each VPCS terminal, test the destination:

```text
ping 8.8.8.8
```

Expected results, assuming routing and the destination are configured correctly:

- PC1: Ping succeeds.
- PC2: Ping fails because of the ACL.
- PC3: Ping succeeds.
- PC4: Ping succeeds.

A failed ping alone does not prove the ACL is working. Confirm the ACL counters and verify routing and destination reachability.

## Key Learnings
- Difference between standard and extended ACLs.
- How ACL rules are processed in sequence.
- How to use `permit` and `deny` statements.
- How to apply ACLs inbound and outbound on router interfaces.
- How to verify traffic filtering using Cisco IOS commands.
- How to troubleshoot connectivity using ping, routing tables, and ACL counters.

## Technologies Used
- Cisco IOS
- GNS3
- Cisco routers and switches
- IPv4 addressing and subnetting
- Extended Access Control Lists

## Conclusion
Successfully designed an ACL-based traffic-filtering lab in GNS3 to demonstrate selective access control. The project illustrates how extended ACLs can restrict communication from a specific host while maintaining connectivity for permitted devices.

## Author
**Sai Rama Reddy**

Networking | Cisco Routing and Switching | Network Troubleshooting