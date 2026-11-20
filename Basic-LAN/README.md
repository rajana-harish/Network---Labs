# Lab 1 - Basic LAN with SSH (GNS3)

This is my first lab in this repo. I built a small network in GNS3 with
one router, two switches and four PCs, split into two subnets. The goal
was to get both subnets talking to each other through the router and then
set up SSH on the router.

Tools: GNS3, Cisco c7200 (IOS 15.3), VPCS, GNS3 Ethernet switch

## Topology

<img width="707" height="458" alt="topology png" src="https://github.com/user-attachments/assets/220f291b-6250-435c-b99c-eb7db1941a07" />
<img width="948" height="560" alt="ssh-test png" src="https://github.com/user-attachments/assets/3b355463-6d34-4c6e-b6a5-5dd1a4237770" />
<img width="769" height="573" alt="sh-ip route png" src="https://github.com/user-attachments/assets/03563a19-fe10-45ec-8911-4fc1a9daecc7" />
<img width="958" height="599" alt="ping-test png" src="https://github.com/user-attachments/assets/5d0eb1f9-1721-47ff-97d2-ba45441afdcc" />

- Switch1 -> R1 Gi1/0, PC1, PC2 (192.168.1.0/24)
- Switch2 -> R1 Gi2/0, PC3, PC4 (192.168.2.0/24)

## IP addresses

| Device | IP | Gateway |
|--------|----|---------|
| R1 Gi1/0 | 192.168.1.100/24 | - |
| R1 Gi2/0 | 192.168.2.100/24 | - |
| PC1 | 192.168.1.1/24 | 192.168.1.100 |
| PC2 | 192.168.1.2/24 | 192.168.1.100 |
| PC3 | 192.168.2.1/24 | 192.168.2.100 |
| PC4 | 192.168.2.2/24 | 192.168.2.100 |

## What I did

1. Gave IPs to the router interfaces and brought them up with 'no shutdown'
2. Set IP and gateway on each PC and saved it with 'save'
3. Set enable secret, a local user and domain name on the router
4. Generated an RSA key and enabled SSH version 2 on the VTY lines
5. Saved the config

## Testing

- PC1 to PC2 (same subnet): ping worked
- PC1 to PC3 (other subnet): ping worked, TTL was 63, so it went
  through R1
- 'show ip interface brief': both interfaces up/up
- 'show ip route': both subnets showed as connected
- 'ssh -l Harish 192.168.1.100' from R1 itself: login worked
  (I have not tested it from another device yet)

Screenshots are in this folder: ping-test.png, show-ip-route.png, ssh-test.png

## Problems I ran into

- 'write' does not work on VPCS, you have to use "save".
- "show ip ssh" first showed version 1.99. After "ip ssh version 2"
  it showed 2.0.

## Notes

- With the PCs pointing to the router as gateway, the router handles
  traffic between the two subnets without any routing config, because
  both networks are directly connected.
- SSH did not work until the domain name, RSA key and local user
  
  were all there.[Google-config.txt.txt](https://github.com/user-attachments/files/35131572/Google-config.txt.txt)
