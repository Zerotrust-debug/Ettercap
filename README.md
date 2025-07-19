# Ettercap
My Experience with Ettercap: Man-in-the-Middle Attack and Packet Interception
Introduction and Plan

I embarked on a hands-on cybersecurity learning journey to understand how Man-in-the-Middle (MITM) attacks work using Ettercap. My aim was to simulate an attack, capture packets, and analyze the kind of information that could be intercepted in a local area network (LAN). Here's a breakdown of the plan I followed:

Setup: Ensure I have two devices in the same network — a Kali Linux machine as the attacker and a Windows PC (later switched to a phone) as the victim.

Enable IP Forwarding: This is critical so that the victim can still access the internet through the attacker's machine.

Use Ettercap: I used Ettercap in text mode for performing ARP spoofing.

Inspect Traffic: Tools like Wireshark were used to observe DNS requests and HTTP traffic.

Step-by-Step Procedure

1. Network Setup

I confirmed the IP addresses of the devices:

Kali Linux (Attacker): <attacker-ip>

Victim Device: Initially <windows-ip>, later switched to a mobile phone <phone-ip>

Gateway: <gateway-ip>

2. Enable IP Forwarding

sudo echo 1 > /proc/sys/net/ipv4/ip_forward

I confirmed IP forwarding was enabled with:

cat /proc/sys/net/ipv4/ip_forward

It returned 1, which is correct.

3. Set Up iptables for NAT

iptables --flush
iptables --table nat --flush
iptables --delete-chain
iptables --table nat --delete-chain
iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE

4. ARP Spoofing with Ettercap

ettercap -T -i eth0 -M arp:remote /<victim-ip>/ /<gateway-ip>/

This command performed ARP poisoning on both the victim and the gateway.

5. Traffic Analysis with Wireshark

I opened Wireshark and began to observe the traffic from the victim. When I opened TikTok on the phone, I was able to intercept DNS queries like:

User Datagram Protocol, Src Port: 56059, Dst Port: 53

This meant the victim was sending DNS requests to a DNS server — a typical behavior of apps initiating connections.

Challenges Faced

ARP Spoof Not Working on Windows: Despite seeing ARP requests, the Windows machine wouldn't accept the spoofed gateway MAC.

Network Connectivity Loss: During spoofing, the victim often lost internet access.

Firewall or Network Behavior: Likely due to aggressive Windows security features or switch behavior.

Mitigations and Fixes

Switch to Phone as Victim: Replacing Windows with a mobile phone helped bypass some of the issues.

Manual ARP Cache Clear: I ran arp -d * to flush ARP entries before spoofing.

Re-confirm IP Forwarding: I checked and re-ran IP forwarding and iptables commands whenever things broke.

Success and Results

Once I used the phone, ARP spoofing was successful. The victim (phone) saw the attacker's MAC as the gateway. I intercepted packets using Wireshark, particularly DNS requests and other app traffic. While HTTPS prevented full data visibility, it was clear that a MITM position had been achieved.

Conclusion

This exercise taught me the nuances of network spoofing and traffic analysis. I learned how OS behavior, network configuration, and even switch models can influence MITM success. The shift from Windows to mobile was crucial in achieving results. Tools like Ettercap and Wireshark, when used in sync with IP forwarding and iptables, can demonstrate how easy it is to intercept data in poorly secured networks. Understanding this reinforces the need for encrypted protocols and proper network segmentation.


