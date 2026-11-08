# SSH Configuration — Cisco Router

In this project, I configured SSH on my Cisco router so I could access and manage it remotely from my computer using PuTTY instead of connecting through the console cable every time.
<img width="336" height="47" alt="Screenshot 2026-11-08 033507" src="https://github.com/user-attachments/assets/7581ac8b-b504-4c59-ba87-fa9fd52c738a" />
<img width="323" height="151" alt="Screenshot 2026-11-08 033449" src="https://github.com/user-attachments/assets/40066182-a7ce-4551-b174-da7845451351" />
<img width="375" height="64" alt="Screenshot 2026-11-08 033431" src="https://github.com/user-attachments/assets/6264cfab-2c29-4f3f-988e-ce0943592b41" />
<img width="658" height="392" alt="Screenshot 2026-11-08 034105" src="https://github.com/user-attachments/assets/dd7b81f4-e19a-477c-a14d-bf316494e3c4" />




I started by configuring the domain name and creating a local username and password for authentication.

```cisco
ip domain-name WEBSTER
username CCNA secret CCNA
```

Next, I configured the console line and disabled the inactivity timeout while working in my lab.

```cisco
line console 0
exec-timeout 0 0
```

After that, I configured the VTY lines to use the local username and password and accept SSH connections.

```cisco
line vty 0 15
login local
transport input ssh
```

I also configured the router hostname and enabled SSH version 2.

```cisco
hostname CCNAROUTER
ip ssh version 2
```

The router needed RSA keys for SSH, so I generated a new 2048-bit RSA key pair.

```cisco
crypto key generate rsa
```

When the router asked for the key modulus, I entered `2048`. The router successfully generated the keys.

I then opened PuTTY on my Windows computer, selected SSH, entered the router's IP address `192.168.1.1`, and used port `22`.

PuTTY connected to the router and prompted me for authentication. After logging in, I was able to access the Cisco command-line interface.

I entered privileged EXEC mode and then global configuration mode.

```cisco
enable
configure terminal
```

The router accepted the commands, and I reached the following prompt:

```text
CCNAROUTER(config)#
```

This confirmed that I could successfully connect to and manage my Cisco router through SSH using PuTTY.

This project helped me understand how to configure SSH, generate RSA keys, set up local authentication, and remotely manage a Cisco router without using a console cable.

All testing was performed in my home networking lab.
