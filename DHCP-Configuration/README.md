# DHCP Configuration

## Project Overview



In this project, I configured a Cisco router to act as a DHCP server for my local network. My goal was to allow my Windows PC to obtain its IP address and other network settings automatically instead of configuring them manually.

After completing the configuration, I renewed the IP address on my PC and verified that it successfully received its network information.

## 1. DHCP Pool Configuration

I created a DHCP pool named `HOME_SERVER` and configured the network address, subnet mask, default gateway, and DNS server.

```cisco
ip dhcp pool HOME_SERVER
network 10.0.0.0 255.255.255.0
default-router 10.0.0.1
dns-server 8.8.8.8
`<img width="305" height="38" alt="Screenshot 2026-11-08 022913" src="https://github.com/user-attachments/assets/03f2b87f-e377-4c0d-96e2-c73a84515a4d" />

I used the following settings:

| Setting | Configuration |
|---|---|
| DHCP Pool | HOME_SERVER |
| Network | 10.0.0.0/24 |
| Subnet Mask | 255.255.255.0 |
| Default Gateway | 10.0.0.1 |
| DNS Server | 8.8.8.8 |
<img width="432" height="84" alt="Screenshot 2026-11-08 022836" src="https://github.com/user-attachments/assets/0d7e5cb7-25ad-44a9-bb7d-4084d7ee82cf" />

## 2. Excluding the Default Gateway

I also excluded the default gateway address from the DHCP address pool.

```cisco
ip dhcp excluded-address 10.0.0.1
```
<img width="453" height="56" alt="Screenshot 2026-11-08 022853" src="https://github.com/user-attachments/assets/8e0631d7-75ed-4086-afae-63ef92beec94" />

This prevents the DHCP server from assigning `10.0.0.1` to a client because that address is reserved for the default gateway.

## 3. Testing DHCP on My Windows PC

After configuring the DHCP server, I renewed the IP address on my Windows PC.

I then checked the network configuration using:

```powershell
ipconfig
```

The PC received the following IPv4 information:

| Setting | Result |
|---|---|
| IPv4 Address | 10.0.0.2 |
| Subnet Mask | 255.255.255.0 |
| Default Gateway | 10.0.0.1 |
<img width="795" height="222" alt="Screenshot 2026-11-08 023135" src="https://github.com/user-attachments/assets/2af8fb8e-8c98-4cbd-b9cc-07dcdbfe9cc9" />

The IPv4 address belonged to the subnet I configured on the router.

## 4. Results

The DHCP configuration was successful.

My Windows PC obtained an IPv4 address automatically after I renewed its network configuration. The assigned IPv4 address, subnet mask, and default gateway matched the network settings I configured.

This test confirmed that my PC could obtain IPv4 network configuration through DHCP.

## 5. Conclusion

This project gave me hands-on experience configuring a DHCP server on a Cisco router and verifying the results from a Windows client.

I practiced creating a DHCP pool, configuring the default gateway and DNS server, excluding a reserved address, and checking the IPv4 settings assigned to a client.
