I built this lab in Cisco Packet Tracer to practice Port Security and DHCP Snooping. I wanted to test what happens when an unauthorized computer connects to a switch and when a fake DHCP server tries to give IP addresses to devices on the network.
Part 1: Port Security
1. Configuring Port Security on Fa0/2
I started by configuring Port Security on FastEthernet0/2. I made a mistake when entering the maximum MAC address command. The switch rejected it, so I corrected the syntax.
interface fa0/2
switchport mode access
switchport port-security
switchport port-security maximum 5
<img width="453" height="367" alt="image" src="https://github.com/user-attachments/assets/0b993d36-8ecb-46cc-99d6-730d38f89ca3" />

After correcting the command, I checked the port again. Port Security was enabled, the port was Secure-up, the maximum was five MAC addresses, and the violation mode was Shutdown.
do show port-security interface fa0/2

<img width="382" height="289" alt="image" src="https://github.com/user-attachments/assets/2bce8763-0f84-4a24-ac31-aa9460c55011" />
Configuring Protect Mode on Fa0/5
I configured FastEthernet0/5 to allow one MAC address and use Protect mode. Protect blocks traffic from unauthorized MAC addresses without shutting down the port.
interface fa0/5
switchport mode access
switchport port-security
switchport port-security aging time 60
switchport port-security violation protect
switchport port-security maximum 1
<img width="409" height="135" alt="image" src="https://github.com/user-attachments/assets/20af5f45-2850-484a-9685-c3581d29efab" />
I verified the configuration to make sure Protect mode was applied.
do show port-security interface fa0/5
<img width="368" height="212" alt="image" src="https://github.com/user-attachments/assets/75f4694c-159f-487a-a8b8-38dab8f0a473" />
<img width="377" height="221" alt="image" src="https://github.com/user-attachments/assets/c5e419e5-8cde-4534-b0ba-7783bf168f26" />
Configuring Sticky MAC Addresses on Fa0/3
I enabled sticky MAC learning on FastEthernet0/3. This allows the switch to learn MAC addresses automatically and add them to the running configuration.
interface fa0/3
switchport port-security mac-address sticky


I checked the port afterward. Port Security was enabled, but the output showed that no sticky MAC addresses had been learned yet.
<img width="507" height="267" alt="image" src="https://github.com/user-attachments/assets/d71b9761-3ee1-429e-9a7c-7fac63278101" />
Configuring Shutdown Mode on Fa0/6
I configured FastEthernet0/6 with a maximum of one MAC address, sticky learning, a 30-minute aging time, and Shutdown mode.
interface fa0/6
switchport mode access
switchport port-security
switchport port-security maximum 1
switchport port-security violation shutdown
switchport port-security aging time 30
switchport port-security mac-address sticky
<img width="419" height="330" alt="image" src="https://github.com/user-attachments/assets/7f176929-cca4-4297-8219-de863f17076d" />
Configuring Port Security on Fa0/4
I applied similar settings on FastEthernet0/4 and verified that Port Security was enabled.
interface fa0/4
switchport mode access
switchport port-security
switchport port-security aging time 30
switchport port-security violation shutdown
switchport port-security maximum 1
switchport port-security mac-address sticky
<img width="435" height="328" alt="image" src="https://github.com/user-attachments/assets/12b7359d-9809-4901-8ecf-b4ac53385132" />
Checking Fa0/1
I checked FastEthernet0/1, which connects to the main router in my topology.

The output showed that Port Security was disabled and the port was Secure-down at that moment. I kept this screenshot because it shows why checking the configuration is important.
do show port-security interface fa0/1
<img width="444" height="302" alt="image" src="https://github.com/user-attachments/assets/ee6d610e-8220-4c63-9386-c7eed34ed7ee" />
Original Network Topology
My network included one router, one switch, four PCs, and one laptop.

The main network used 192.168.1.0/24, with 192.168.1.1 assigned to the router.
<img width="744" height="560" alt="image" src="https://github.com/user-attachments/assets/689576f0-039a-48fd-bcd6-d3e8c95544d2" />
Testing an Unauthorized PC
After configuring Port Security, I connected another PC to the switch to simulate an unauthorized device.

The new connection showed red indicators in Packet Tracer, and the link was not operational.

This gave me a way to test how the switch responds when another device is connected. To confirm that Port Security caused the link to go down, I would also need to check the violation counter and interface status after the test.
<img width="800" height="551" alt="image" src="https://github.com/user-attachments/assets/b84bb3be-5377-4d47-87d9-dd086241ea27" />
Part 2: DHCP Snooping
9. Configuring DHCP Snooping
Next, I worked on DHCP Snooping to protect the network from unauthorized DHCP servers.

I enabled DHCP Snooping globally and for VLAN 1. I also disabled Option 82 insertion and configured FastEthernet0/1 as a trusted port.
ip dhcp snooping
ip dhcp snooping vlan 1
no ip dhcp snooping information option

interface fa0/1
ip dhcp snooping trust


I checked the configuration, and the switch showed that DHCP Snooping was enabled for VLAN 1 and Fa0/1 was trusted.
<img width="455" height="271" alt="image" src="https://github.com/user-attachments/assets/f716b166-5f99-4d26-b252-671c998a0e22" />
Configuring the DHCP Server
For this DHCP test, I used the 10.0.0.0/24 network.

I configured a DHCP pool named SERVER2 and excluded 10.0.0.1 from the available addresses.
ip dhcp excluded-address 10.0.0.1

ip dhcp pool SERVER2
network 10.0.0.0 255.255.255.0
default-router 10.0.0.1
<img width="446" height="117" alt="image" src="https://github.com/user-attachments/assets/21bf4e89-282c-4216-9fcd-2acdf8552139" />
<img width="352" height="68" alt="image" src="https://github.com/user-attachments/assets/5c7d49b2-011e-4a83-acc4-a342b50830ad" />
11. Testing DHCP on the Client
I renewed the IP address on the client.
ipconfig /renew


The client received:
- IP address: 10.0.0.2
- Subnet mask: 255.255.255.0
- Default gateway: 10.0.0.1
This confirmed that DHCP was assigning addresses during this test.
<img width="394" height="94" alt="image" src="https://github.com/user-attachments/assets/6c3a9736-a724-4cc4-b007-1810d4228a18" />
Correcting a DHCP Snooping Command Error
While configuring DHCP Snooping, I entered ip dhcp snooping vlan 1 under interface configuration mode.

The switch rejected the command because it needed to be entered in global configuration mode.

I corrected it and enabled DHCP Snooping successfully.
Switch(config)# ip dhcp snooping vlan 1
Switch(config)# ip dhcp snooping
<img width="493" height="206" alt="image" src="https://github.com/user-attachments/assets/aa5356b5-b98b-46eb-bc23-536a644d5e54" />
Testing a Fake DHCP Server
I connected another router to simulate a fake DHCP server.

Before enabling DHCP Snooping, I tested the fake server and observed that it could provide DHCP service.

After enabling DHCP Snooping, I tested again.

This time, PC3 displayed DHCP request failed.

The screenshot shows the failed request. To confirm DHCP Snooping was responsible, the fake server needs to be connected to an untrusted port, while the legitimate DHCP server remains reachable through a trusted port.
<img width="837" height="203" alt="image" src="https://github.com/user-attachments/assets/9a329797-89d5-4a6f-9fd6-0ba185ed5835" />
What I Learned
This project helped me understand how Port Security and DHCP Snooping work on Cisco switches.

With Port Security, I practiced setting maximum MAC addresses, enabling sticky MAC learning, configuring Protect and Shutdown modes, and connecting another PC to test the switch.

With DHCP Snooping, I learned how to configure trusted ports, enable protection for VLAN 1, and test the behavior of a fake DHCP server.

I also made configuration mistakes during the lab and corrected them. That helped me understand the commands better.

The most important thing I learned was how to test a configuration and check the results instead of assuming that everything works just because the commands were accepted.
















