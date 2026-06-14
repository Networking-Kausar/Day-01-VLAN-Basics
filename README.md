# CCNA-LAB-30-DAY-SERIES-01-VLAN
# CCNA Lab Day 1 – VLAN Basics

## Overview

This lab introduces the basic concept of VLANs (Virtual Local Area Networks). The goal is to create two separate VLANs on a switch and understand how devices in different VLANs are isolated from each other.

## Objectives

After completing this lab, you will be able to:

- Create VLANs on a Cisco switch
- Assign switch ports to VLANs
- Verify VLAN configuration
- Test communication within and across VLANs
- Understand VLAN isolation

## Topology

```text
      PC1
       |
Fa0/1  |
       |
      SW1
       |
Fa0/2  |
      PC2


      PC3
       |
Fa0/3  |
       |
      SW1
       |
Fa0/4  |
      PC4
```

## Devices Used

- 1 Cisco Switch (SW1)
- 4 PCs
- Copper Straight-Through Cables

## Connections

| Device | Interface | Connected To |
|----------|----------|----------|
| PC1 | Fa0 | SW1 Fa0/1 |
| PC2 | Fa0 | SW1 Fa0/2 |
| PC3 | Fa0 | SW1 Fa0/3 |
| PC4 | Fa0 | SW1 Fa0/4 |

## IP Addressing

### VLAN 10 (HR)

| Device | IP Address |
|----------|----------|
| PC1 | 192.168.10.10/24 |
| PC2 | 192.168.10.20/24 |

### VLAN 20 (Finance)

| Device | IP Address |
|----------|----------|
| PC3 | 192.168.20.10/24 |
| PC4 | 192.168.20.20/24 |

## Configuration Steps

### Step 1: Verify Existing VLANs

```bash
enable
show vlan brief
```

By default, all switch ports belong to VLAN 1.

### Step 2: Create VLAN 10

```bash
configure terminal

vlan 10
name HR
```

Verify:

```bash
show vlan brief
```

### Step 3: Create VLAN 20

```bash
vlan 20
name FINANCE
```

Verify again:

```bash
show vlan brief
```

### Step 4: Assign Ports to VLAN 10

```bash
interface range fa0/1 - 2
switchport mode access
switchport access vlan 10
```

### Step 5: Assign Ports to VLAN 20

```bash
interface range fa0/3 - 4
switchport mode access
switchport access vlan 20
```

### Step 6: Verify VLAN Membership

```bash
show vlan brief
```

Expected result:

- VLAN 10 → Fa0/1, Fa0/2
- VLAN 20 → Fa0/3, Fa0/4

## Connectivity Testing

### Test 1: PC1 to PC2

```bash
ping 192.168.10.20
```

Result: Success

### Test 2: PC3 to PC4

```bash
ping 192.168.20.20
```

Result: Success

### Test 3: PC1 to PC3

```bash
ping 192.168.20.10
```

Result: Failed

## Why Did the Ping Fail?

VLAN 10 and VLAN 20 are separate Layer 2 networks.

The switch does not forward traffic between VLANs. Devices in different VLANs require a router or Layer 3 switch for communication.

## Data Flow

### Communication Within VLAN 10

```text
PC1
 |
SW1 (VLAN 10)
 |
PC2
```

Traffic is forwarded successfully.

### Communication Between VLANs

```text
PC1
 |
SW1 (VLAN 10)

X

SW1 (VLAN 20)
 |
PC3
```

Traffic is blocked because the devices belong to different VLANs.

## Verification Commands

### Display VLAN Information

```bash
show vlan brief
```

### View Running Configuration

```bash
show running-config
```

### Check Port VLAN Assignment

```bash
show interfaces fa0/1 switchport
```

## Troubleshooting Practice

### Scenario 1: Incorrect VLAN Assignment

Move Fa0/1 into VLAN 20.

```bash
interface fa0/1
switchport access vlan 20
```

Expected Result:

PC1 can no longer communicate with PC2 because they are in different VLANs.

### Scenario 2: Delete VLAN 20

```bash
no vlan 20
```

Observe what happens to ports Fa0/3 and Fa0/4.

### Scenario 3: Shutdown a Port

```bash
interface fa0/2
shutdown
```

Expected Result:

PC1 cannot communicate with PC2 until the interface is re-enabled.

## Skills Learned

- VLAN Creation
- VLAN Naming
- Access Port Configuration
- VLAN Verification
- VLAN Isolation
- Basic Troubleshooting

## Mini Challenge

Create the following VLANs:

| VLAN | Name |
|--------|--------|
| 30 | SALES |
| 40 | IT |

Assign:

| Interface | VLAN |
|-----------|------|
| Fa0/5 | VLAN 30 |
| Fa0/6 | VLAN 40 |

Verify your work using:

```bash
show vlan brief
```

## Conclusion

This lab provided hands-on experience with VLAN creation and management. VLANs are a fundamental technology used in enterprise networks to separate departments, improve security, and reduce unnecessary broadcast traffic.

## Next Lab

Day 2 – Multi-Switch VLAN Network and Trunking Fundamentals

## Author

Muhammad Kausar

CCNA SRWE Hands-On Lab Series
