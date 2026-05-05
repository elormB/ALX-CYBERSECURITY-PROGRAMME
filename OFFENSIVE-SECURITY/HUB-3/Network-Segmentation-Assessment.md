# Network Segmentation Assessment Lab (nmap usage)
## 1. Background

Network segmentation is a critical security practice that divides a network into smaller, isolated zones to control traffic flow, enforce access policies, and reduce the overall attack surface. By separating networks such as the Internal, DMZ, and Management segments, organizations can prevent unauthorized lateral movement and ensure that sensitive systems are protected from less secure areas. This layered approach enhances security by containing potential breaches and limiting the impact of attacks.

In this lab, a simulated corporate environment is used to assess how effectively segmentation is implemented across different network zones. Using tools like Nmap along with utilities such as traceroute and routing table analysis, connectivity between segments is tested and analyzed. The lab focuses on identifying whether proper isolation exists, how traffic is controlled between VLANs, and how network responses (such as timeouts or blocked connections) reflect the presence of security mechanisms like firewalls and access control rules.

## 2. Objective

The objective of this task is to:

- Test connectivity within the internal network (192.168.10.x).
- Attempt access to other segments (DMZ: 192.168.20.x, Management: 192.168.30.x).
- Analyze routing tables to understand network structure
- Use traceroute to map network paths.
- Determine whether proper network isolation and segmentation controls are in place.

## 3. Scope and Limitations
### Scope
- Ths lab is limited to the simulated lab environment
- This lab focuses on three network segments:
    - Internal (192.168.10.0/24)
    - DMZ (192.168.20.0/24)
    - Management (192.168.30.0/24)
- This lab includes connectivity testing, routing analysis, and path tracing
### Limitations

- No exploitation or privilege escalation is performed
- Results are based on a simulated network and may not reflect real-world complexity
- Firewall rules and segmentation controls are preconfigured and may limit visibility.

## 4. Legal and Ethical Disclaimer

- This assessment is conducted within an authorized and controlled lab environment for educational purposes only.
- All activities must remain within the provided network ranges. 
- Unauthorized attempts to scan or access external systems are strictly prohibited and may violate legal and ethical standards in cybersecurity.

## 5. Lab Initialization and Access

To begin the practical, the lab was confirmed to be active and accessible:

1. The training-shell terminal was launched and Hub 3 was started.
2. The running command was executed to validate that the environment was live: running # expected output: hub-3 running.
3. The browser was opened and the interface was accessed via http://localhost
4. Navigation proceeded to the Network segmentation assessment section.
5. All the required tasks were initiated and completed

## 6. Challenge Overview

This task focuses on evaluating whether proper segmentation exists between network zones.

Activities include:

1. Testing communication within the internal network
2. Attempting access to DMZ and Management segments
3. Identifying blocked or allowed traffic
4. Analyzing network responses (timeouts, refusals, successful connections)


## 7. Tools and Prerequisites

### Prerequisites
- Basic understanding of IP networking and subnetting.
- Familiarity with command-line tools.
- Knowledge of network security concepts such as VLANs and firewalls.
  
### Tools Used

| Tool | Purpose |
|------|--------|
| Nmap | Connectivity testing and host discovery |
| traceroute | Mapping network paths and identifying routing behavior |
| netstat / ip route | Viewing routing tables and network configuration |
| Kali Linux Terminal | Execution environment |


### IP Ranges Provided in the Lab

| Network Segment     | IP Range              | Purpose                                      |
|--------------------|----------------------|----------------------------------------------|
| Internal Network   | 192.168.10.0/24      | Startiing point i.e. Current network on which attacker is connected      |
| DMZ Network        | 192.168.20.0/24      | Public-facing or semi-trusted systems        |
| Management Network | 192.168.30.0/24      | Restricted administrative systems            |


## 8. Tasks Completed

### 8.1 Ping Test
A ping test is a basic network diagnostic method used to determine whether a host is reachable and how quickly it responds. It works by sending ICMP Echo Request packets (operating  on OSI layer 3) to a target system and waiting for ICMP Echo Reply responses. A successful reply indicatea that the host is online and accessible, while metrics such as response time and packet loss provide insight into network performance and stability. 
Overall, ping is commonly used to verify connectivity and assess the reliability of communication within a network.

### 8.1.1 Testing connectivity to target in current network segment 192.168.10.0/24

The purpose of this task is to verify network connectivity between the attacking machine and a host within the same internal network segment (192.168.10.0/24). By using the ping command, it confirms whether the target host (192.168.10.50) is reachable and responsive over the network.

The goal is to ensure that communication within the internal network is functioning correctly, establish that the host is active, and validate a baseline for further network segmentation testing and reconnaissance activities.

#### Checking for attacker Ip address

```bash
Pentester@kali-linux:~$ ifconfig
eth0: flags=4163  mtu 1500
        inet 192.168.10.100  netmask 255.255.255.0  broadcast 192.168.10.255
        inet6 fe80::20c:29ff:fe1a:2b3c  prefixlen 64  scopeid 0x20
        ether 00:0c:29:1a:2b:3c  txqueuelen 1000  (Ethernet)
```

#### Performing Ping test on target 192.168.10.50
```bash
pentester@kali-linux:~$ ping 192.168.10.50
PING 192.168.10.50 (192.168.10.50) 56(84) bytes of data.
64 bytes from 192.168.10.50: icmp_seq=1 ttl=64 time=0.432 ms
64 bytes from 192.168.10.50: icmp_seq=2 ttl=64 time=0.387 ms
^C
--- 192.168.10.50 ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1001ms
pentester@kali-linux:~$
```
#### Conclusion

The ping test between 192.168.10.100 and 192.168.10.50 was successful, confirming that both hosts are active and reachable within the same internal network segment. The output shows 0% packet loss and very low response times (around 0.4 ms), indicating stable and reliable communication. This result verifies that internal connectivity is functioning properly and establishes a baseline for further network scanning and segmentation testing.


### 8.1.2 Test connectivity to target in DMZ network segment 192.168.20.0/24

```bash
Pentester@kali-linux:~$ ping 192.168.20.10
PING 192.168.20.10 (192.168.20.10) 56(84) bytes of data.
^C
--- 192.168.20.10 ping statistics ---
3 packets transmitted, 0 received, 100% packet loss, time 2034ms
pentester@kali-linux:~$
```

#### Conclusion

The ping test between **192.168.10.100** and **192.168.20.10 (DMZ network)** was unsuccessful, as all packets were lost with **100% packet loss** and no responses received. This indicates that the target is either unreachable, actively blocking ICMP traffic, or properly isolated by network segmentation controls such as firewalls or VLAN restrictions. The result demonstrates that communication between the internal network and the DMZ is restricted, confirming that segmentation policies are likely in place to prevent unauthorized cross-network access.


### 8.1.3 Test connectivity to target in Managment network segment 192.168.30.0/24

```bash

pentester@kali-linux:~$ ping 192.168.30.5
PING 192.168.30.5 (192.168.30.5) 56(84) bytes of data.
From 192.168.10.1 icmp_seq=1 Destination Host Prohibited
From 192.168.10.1 icmp_seq=2 Destination Host Prohibited
^C
--- 192.168.30.5 ping statistics ---
2 packets transmitted, 0 received, +2 errors, 100% packet loss, time 1005ms
pentester@kali-linux:~$
```
#### Conclusion

The ping test to 192.168.30.5 (Management network) was unsuccessful, as the system returned “Destination Host Prohibited” and resulted in 100% packet loss. This indicates that the target network is actively blocking ICMP traffic or enforcing strict access control policies between the internal and management segments. The response confirms that segmentation controls are in place, preventing unauthorized communication and ensuring that the management network remains isolated and secure from the internal network.


### 8.2 Analyzing routing tables to understand network structure

Analyzing routing tables is a key network reconnaissance technique used to understand how data moves within and between networks. A routing table shows the paths that network traffic follows, including available gateways, directly connected networks, and preferred routes for reaching specific IP ranges. By reviewing this information using tools such as ```ip route``` or ``netstat -r``, a security analyst can identify how different network segments (Internal, DMZ, and Management) are interconnected.

In a segmentation assessment, routing tables help reveal whether separate VLANs are properly isolated or if routing paths exist that could allow unintended communication. They also highlight default gateways and potential choke points where traffic is filtered or controlled. 

Overall, routing analysis provides critical insight into network topology and helps identify possible attack paths or misconfigurations in network.

```bash
pentester@kali-linux:~$ route -n
Kernel IP routing table
Destination     Gateway         Genmask         Flags Metric Ref    Use Iface
0.0.0.0         192.168.10.1    0.0.0.0         UG    100    0        0 eth0
192.168.10.0    0.0.0.0         255.255.255.0   U     100    0        0 eth0
192.168.10.1    0.0.0.0         255.255.255.255 UH    100    0        0 eth0
```

#### Interpretation

The routing table output shows that the host is directly connected to the 192.168.10.0/24 internal network via the eth0 interface, meaning all traffic within this subnet is routed locally without needing a gateway. The default gateway is set to 192.168.10.1, which handles all traffic destined for external networks outside the local subnet.

In a routing table, the Flags column indicates the status and type of each route. The flag U means the route is active and usable, while G indicates that the route uses a gateway or next-hop router to forward traffic outside the local network. The H flag represents a host-specific route, meaning it applies to a single IP address rather than an entire network. In the given output, UG shows the default route through a gateway (used for external traffic), U indicates a directly connected network that does not require a gateway, and UH represents a specific host route that is active and reachable.

No routes are present for the DMZ (192.168.20.0/24) or Management (192.168.30.0/24) networks, indicating that access to these segments is not directly defined at the routing level. This suggests that communication to those networks would depend on the default gateway and any firewall or segmentation policies in place. Overall, the routing table confirms a structured internal network with controlled external routing and no direct inter-segment routing visibility.



### 8.3 Using traceroute to map network paths

Traceroute is a network diagnostic tool used to track the path that data packets take from a source device to a destination host. In Nmap environments and general networking, it works by sending packets with gradually increasing Time-To-Live (TTL) values. Each router along the path reduces the TTL by one; when the TTL reaches zero, the router sends back an error message, revealing its address. This process continues until the packet reaches the destination, allowing traceroute to map every hop between the source and target. It is commonly used to identify network routes, measure latency, and detect where delays or blocks occur in a network path.

```bash
pentester@kali-linux:~$ traceroute 192.168.10.50
traceroute to 192.168.10.50 (192.168.10.50), 30 hops max, 60 byte packets
 1  192.168.10.50 (192.168.10.50)  0.432 ms  0.387 ms  0.356 ms
pentester@kali-linux:~$-
```

#### Interpretation

The traceroute to 192.168.10.50 shows that the destination was reached in a single hop, indicating that the target host is located within the same local network segment (192.168.10.0/24) as the source machine. The very low response times confirm direct and efficient communication without any intermediate routers or network devices. This result demonstrates that internal network traffic flows directly between hosts within the same subnet, with no routing barriers or segmentation between them in this path.

## 9. Key Findings and Risk Table

| Tool Used | Technique Type | Command(s) Executed | Key Output / Observed Result | Insight Gained | Risk Level | Lessons Learned | Recommended Action / Mitigation | Conclusion |
|-----------|----------------|---------------------|------------------------------|----------------|------------|-----------------|----------------------------------|------------|
| Ping (ICMP) | Host Connectivity Test | ping 192.168.10.50 | 0% packet loss, very low latency (~0.4 ms) | Host is active and reachable within same subnet | Low | Internal hosts communicate freely within LAN | Monitor internal traffic for lateral movement risks | Internal connectivity is functional |
| Ping (ICMP) | Segmentation Test | ping 192.168.20.10 | 100% packet loss, no response | DMZ host is not reachable from internal network | Medium | ICMP traffic is blocked or network is isolated | Enforce proper firewall rules for controlled access | DMZ is isolated |
| Ping (ICMP) | Segmentation Test | ping 192.168.30.5 | “Destination Host Prohibited” | Management network is actively blocked at gateway | High | Strong segmentation prevents access to critical systems | Maintain strict ACLs and firewall restrictions | Management network is fully restricted |
| Routing Analysis | Network Structure Review | route -n | Default gateway 192.168.10.1, no routes to other segments | Network relies on gateway for external communication | Medium | Routing table defines local vs external traffic flow | Review routing and segmentation policies regularly | Internal routing is properly structured |
| Traceroute | Path Discovery | traceroute 192.168.10.50 | Single hop reached successfully | Target is within same subnet, no intermediate devices | Low | Same subnet communication is direct | Monitor internal network segmentation | Direct internal path confirmed |


## 10. Troubleshooting
When commands failed or services did not respond:

- The current shell was validated using the command `echo $SHELL`
- If incorrect, the **start_script.sh** was re-run
- The lab state was verified again using `running`

This ensured tool execution remained within the designated training environment.


## 11. Summary and Lesson Learnt
### Summary

- The lab demonstrated fundamental network reconnaissance techniques using tools such as ping, routing analysis, and traceroute to assess connectivity and network segmentation. 
- Internal communication within the 192.168.10.0/24 network was successful, confirming active hosts and stable connectivity. 
- However, attempts to reach the DMZ (192.168.20.0/24) and Management (192.168.30.0/24) networks were unsuccessful, indicating that segmentation controls and firewall rules are effectively restricting cross-network communication. 
- Routing and path analysis further confirmed that traffic to external segments is controlled through a default gateway, with no direct routes to restricted networks.

### Lessons Learned

- This lab reinforced the importance of network segmentation in securing enterprise environments. 
- It demonstrated how basic tools like ping and traceroute can reveal connectivity patterns, host availability, and network boundaries. 
- A key lesson is that successful internal connectivity does not imply unrestricted access across networks, as segmentation and firewall policies play a critical role in enforcing isolation. 
- Additionally, interpreting routing tables and traceroute results is essential for understanding network topology and identifying potential security controls or attack barriers.

## 12. Conclusion

This lab successfully demonstrated how network reconnaissance tools such as ping, routing analysis, and traceroute can be used to evaluate connectivity and assess network segmentation. The results confirmed that devices within the internal network (192.168.10.0/24) communicate effectively, while access to the DMZ (192.168.20.0/24) and Management (192.168.30.0/24) networks is restricted through segmentation controls and firewall policies. Overall, the findings show that the network is properly segmented, with controlled routing and enforced isolation between security zones, reducing the risk of unauthorized lateral movement