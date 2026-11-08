# NAT/PAT Configuration

In this project, I configured NAT/PAT on my Cisco 2921 router to allow devices on my private lab network to use the connection from my home router.

I started by checking the IP addresses and status of my router interfaces.

```cisco
show ip interface brief
```

I confirmed that GigabitEthernet0/0 had the IP address `192.168.1.1` and was up. This is the interface connected to my lab network.

GigabitEthernet0/2 was also up and received its IP address through DHCP. This interface connects to my home router, which provides the outside connection.

I then configured G0/0 as my inside NAT interface.

```cisco
interface g0/0
ip nat inside
```

Next, I configured G0/2 as my outside NAT interface.

```cisco
interface g0/2
ip nat outside
```

After that, I created an access list to identify the IP addresses on my lab network that I wanted NAT to translate.

```cisco
access-list 20 permit 192.168.1.0 0.0.0.255
```

Finally, I configured PAT using the outside interface.

```cisco
ip nat inside source list 20 interface g0/2 overload
```

I used the `overload` command because I wanted multiple devices on my lab network to share the IP address assigned to G0/2.

This completed the NAT/PAT configuration steps. My next step was to verify the translations and test connectivity from a device on my lab network.

<img width="733" height="253" alt="Screenshot 2026-11-08 031542" src="https://github.com/user-attachments/assets/928617ef-3f05-4a6e-9221-9cf0218ffc5a" />



