Why I did this lab
I built this network in Cisco Packet Tracer to practice finding out which devices are connected to which switch ports. In a larger network, a new engineer might take over without knowing where every cable goes. I wanted to see how I could use the information from the devices themselves to identify the connections instead of guessing.

I started with a switch, a router, four PCs, and a laptop. My first topology had the devices connected but did not show the switch port numbers or IP addresses.
<img width="976" height="457" alt="image" src="https://github.com/user-attachments/assets/19bff6ad-930b-4604-95d7-6eb55a3411bc" />
Checking the IP addresses
I opened the command prompt on the end devices and ran:


ipconfig

Two of the PCs showed 192.168.1.4 and 192.168.1.5, both with a 255.255.255.0 subnet mask and 192.168.1.1 as their default gateway.
<img width="998" height="630" alt="image" src="https://github.com/user-attachments/assets/ca673276-29df-4a7e-9117-ac843d7e732d" />
<img width="970" height="623" alt="image" src="https://github.com/user-attachments/assets/4c610c8a-4868-47b7-acfe-07b9b87dcc16" />
Another PC initially had an automatic private address, 169.254.58.12, with no usable default gateway. I kept that result because it showed that not every device had received a normal address at that point in the lab.

<img width="929" height="613" alt="image" src="https://github.com/user-attachments/assets/23550a11-6a25-4102-a48c-1dde2cb3f6a0" />
The laptop showed 192.168.1.2/24 with 192.168.1.1 as the default gateway.

<img width="1027" height="680" alt="image" src="https://github.com/user-attachments/assets/3b7e0356-7704-4d99-b942-c9518fe197fb" />
Using CDP to find a neighboring device
On the router, I ran:


show cdp neighbors

The output showed a Cisco 2960 switch connected to the router's GigabitEthernet0/0 interface. The switch-side port was FastEthernet0/1. This gave me a way to confirm the connection between the router and switch from the CLI.
<img width="931" height="229" alt="image" src="https://github.com/user-attachments/assets/b14a1f8a-e5eb-4e14-8875-c9cb4e9174c9" />
Checking the switch MAC address table
Next, I wanted to identify the devices connected to the switch ports. I started with:


show mac address-table interface f0/2

At first, I did not get the entry I was looking for. After generating traffic by pinging another PC, I checked again and could see a dynamically learned MAC address on Fa0/2.

<img width="943" height="309" alt="image" src="https://github.com/user-attachments/assets/6f76b91f-cc5d-4998-a32e-e577b522cf77" />
I also checked the laptop's network settings to compare its MAC address with the address learned by the switch.
<img width="1020" height="238" alt="image" src="https://github.com/user-attachments/assets/c88b28c0-b5af-4988-af09-2fe6d4c07423" />
I repeated the process for another port, Fa0/3, and saw a dynamic MAC address there as well.
show mac address-table interface f0/3
<img width="964" height="306" alt="image" src="https://github.com/user-attachments/assets/413393e4-30a7-4cee-bef7-2863b0e23531" />
<img width="913" height="343" alt="image" src="https://github.com/user-attachments/assets/5a35d8b2-9124-43ea-96f8-4a89d57a1426" />
A faster way to check the learned addresses
Instead of checking every interface one at a time, I used:
show mac address-table dynamic

This showed the dynamically learned MAC addresses and their associated switch ports in one place. It was much easier to work from this output when I was trying to label the topology.
<img width="853" height="356" alt="image" src="https://github.com/user-attachments/assets/fa59c2e7-1032-461b-a992-4cf79ffe49d5" />
<img width="771" height="344" alt="image" src="https://github.com/user-attachments/assets/a2b0752b-4071-49b1-b45f-ea4131292e1a" />
One thing I learned is that the MAC address table is built from source MAC addresses in frames received by the switch. If a device has not sent traffic recently, its address might not appear in the table. Pinging helped generate traffic so I could check the entries again.

Final topology
After checking the IP configurations, CDP neighbor information, and learned MAC addresses, I added labels to my Packet Tracer topology. The final diagram shows the subnet, device IP addresses, and switch port connections.<img width="882" height="420" alt="image" src="https://github.com/user-attachments/assets/cd30f01a-348d-4d62-8b9f-59fb1c6cda27" />
What I learned
CDP helped me identify the directly connected Cisco switch and the ports used between the router and switch. The MAC address table helped me find which switch ports had learned addresses from the end devices. I also learned that checking a single port is useful when investigating one connection, while show mac address-table dynamic is more convenient when checking several devices.

I named this project CDP/LLDP because both protocols are used for neighbor discovery. The command evidence in this lab is from CDP; I did not capture an LLDP test. The MAC address table checks are a separate Layer 2 method for identifying where end-device MAC addresses were learned.










