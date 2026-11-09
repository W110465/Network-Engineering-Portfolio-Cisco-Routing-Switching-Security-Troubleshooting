CDP/LLDP — Network Device and Port Discovery
Why I did this lab
I built this network in Cisco Packet Tracer to practice finding out which devices are connected to which switch ports. In a larger network, a new engineer might take over without knowing where every cable goes. I wanted to see how I could use the information from the devices themselves to identify the connections instead of guessing.

I started with a switch, a router, four PCs, and a laptop. My first topology had the devices connected but did not show the switch port numbers or IP addresses.

Original network topology
Original network topology
Checking the IP addresses
I opened the command prompt on the end devices and ran:


ipconfig

Two of the PCs showed 192.168.1.4 and 192.168.1.5, both with a 255.255.255.0 subnet mask and 192.168.1.1 as their default gateway.

PC showing 192.168.1.4
PC showing 192.168.1.4
PC showing 192.168.1.5
PC showing 192.168.1.5
Another PC initially had an automatic private address, 169.254.58.12, with no usable default gateway. I kept that result because it showed that not every device had received a normal address at that point in the lab.

PC showing an automatic private IP address
PC showing an automatic private IP address
The laptop showed 192.168.1.2/24 with 192.168.1.1 as the default gateway.

Laptop IP configuration
Laptop IP configuration
Using CDP to find a neighboring device
On the router, I ran:


show cdp neighbors

The output showed a Cisco 2960 switch connected to the router's GigabitEthernet0/0 interface. The switch-side port was FastEthernet0/1. This gave me a way to confirm the connection between the router and switch from the CLI.

Router CDP neighbors output
Router CDP neighbors output
Checking the switch MAC address table
Next, I wanted to identify the devices connected to the switch ports. I started with:


show mac address-table interface f0/2

At first, I did not get the entry I was looking for. After generating traffic by pinging another PC, I checked again and could see a dynamically learned MAC address on Fa0/2.

MAC address table for Fa0/2
MAC address table for Fa0/2
I also checked the laptop's network settings to compare its MAC address with the address learned by the switch.

Laptop network adapter MAC information
Laptop network adapter MAC information
I repeated the process for another port, Fa0/3, and saw a dynamic MAC address there as well.


show mac address-table interface f0/3

MAC address table for Fa0/3
MAC address table for Fa0/3
As I generated more traffic, more addresses appeared in the switch table.

MAC address table with two learned entries
MAC address table with two learned entries
A faster way to check the learned addresses
Instead of checking every interface one at a time, I used:


show mac address-table dynamic

This showed the dynamically learned MAC addresses and their associated switch ports in one place. It was much easier to work from this output when I was trying to label the topology.

Dynamic MAC address table with three entries
Dynamic MAC address table with three entries
After more traffic, I checked again and saw additional learned addresses.

Dynamic MAC address table with four entries
Dynamic MAC address table with four entries
One thing I learned is that the MAC address table is built from source MAC addresses in frames received by the switch. If a device has not sent traffic recently, its address might not appear in the table. Pinging helped generate traffic so I could check the entries again.

Final topology
After checking the IP configurations, CDP neighbor information, and learned MAC addresses, I added labels to my Packet Tracer topology. The final diagram shows the subnet, device IP addresses, and switch port connections.

Final labeled network topology
Final labeled network topology
| Device | IP address shown on final topology | Switch port |

|---|---|---|

| Laptop1 | 192.168.1.2 | Fa0/2 |

| PC1 | 192.168.1.4 | Fa0/5 |

| PC2 | 192.168.1.5 | Fa0/3 |

| PC3 | 192.168.1.6 | Fa0/6 |

| PC4 | 192.168.1.3 | Fa0/4 |

| Main Router | 192.168.1.1 | Fa0/1 (router G0/0) |

The final diagram shows PC4 as 192.168.1.3. Earlier in the lab, one PC had shown a 169.254.x.x address; I have included both screenshots rather than leaving out that earlier result.

What I learned
CDP helped me identify the directly connected Cisco switch and the ports used between the router and switch. The MAC address table helped me find which switch ports had learned addresses from the end devices. I also learned that checking a single port is useful when investigating one connection, while show mac address-table dynamic is more convenient when checking several devices.

I named this project CDP/LLDP because both protocols are used for neighbor discovery. The command evidence in this lab is from CDP; I did not capture an LLDP test. The MAC address table checks are a separate Layer 2 method for identifying where end-device MAC addresses were learned.
