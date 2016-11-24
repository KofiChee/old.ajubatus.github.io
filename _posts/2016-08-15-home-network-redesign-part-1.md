---
excerpt: "For part 1, I wiped the 2901 and began setting up a basic configuration"
---

For part 1, I started with double checking the 2901 was in a working condition and wiping it of the previous owner's information. I was shocked to find the routing information and configuration from the networking company that it used to belong to was still present on the device. After wiping, I set the hostname and began working on a basic configuration.

I started with addressing and on-lining the interfaces. I then added a default route for all traffic destined for the internet.

### Interface setup:

    Dunbar-Router(config)#interface gigabitEthernet 0/0  
    Dunbar-Router(config-if)#ip address 192.168.1.101 255.255.255.0  
    Dunbar-Router(config-if)#ip nat outside
    
    Dunbar-Router(config)#interface GigabitEthernet0/1  
    Dunbar-Router(config-if)#ip address 192.168.0.1 255.255.255.0  
    Dunbar-Router(config-if)#ip nat inside  



### DHCP setup:

    Dunbar-Router(config)#ip dhcp pool DHCPPOOL  
    Dunbar-Router(dhcp-config)#network 192.168.0.0 255.255.255.0  
    Dunbar-Router(dhcp-config)#default  
    Dunbar-Router(dhcp-config)#default-router 192.168.0.1  
    Dunbar-Router(dhcp-config)#dns-s  
    Dunbar-Router(dhcp-config)#dns-server 8.8.8.8 8.8.4.4  
    Dunbar-Router(dhcp-config)#lease 0 12 0

    Dunbar-Router(config)#ip dhcp excluded-address 192.168.0.1 192.168.0.99

### PAT and default route setup:

    Dunbar-Router(config)#interface gigabitEthernet 0/0  
    Dunbar-Router(config-if)#ip nat outside

	Dunbar-Router(config)#interface gigabitEthernet 0/1  
    Dunbar-Router(config-if)#ip nat inside

    Dunbar-Router(config)#access-list 1 permit 192.168.0.0 0.0.0.255  
    Dunbar-Router(config)#ip nat inside source list 1 interface gigabitEthernet 0/0 overload
    Dunbar-Router(config)#ip route 0.0.0.0 0.0.0.0 gigabitEthernet 0/0
    
I then set up some static port translations (port forwarding). This is used for the numerous services I run on my media server, allowing me external access to these services.

### Port forwards:

	Dunbar-Router(config)#ip nat inside source static tcp $serverAddress $serverPort $outsideInterfaceAddress $extendablePort extendable  

For this router I am using a local user for authentication

Next I create a local user and enable SSH on the vty lines, then secure it with a password:

### Local user and SSH:

    Dunbar-Router(config)#username $username password $password
    Dunbar-Router(config)#enable secret

    Dunbar-Router(config)#crypto key generate rsa

    Dunbar-Router(config)#line vty 0 4  
    Dunbar-Router(config-line)#transport input ssh  
    Dunbar-Router(config-line)#login local  
    Dunbar-Router(config-line)#password $password
    Dunbar-Router(config-line)#exit

    Dunbar-Router(config)#line console 0  
    Dunbar-Router(config-line)# logging synchronous  
    Dunbar-Router(config-line)# login local

With these basic steps completed, I then moved the router the the LACKRACK I had previously constructed. This is an IKEA LACK table, with IKEA BYGEL towel racks screwed into the legs. This provides a cheap alternative to a dedicated rack. In the future, I plan to purchase a more suitable rack, but this will do for now.

In the future, I will take a look back at the security and further harden the router. As it stands right now, the router is complete overkill for my home network and should last longer than all the devices in the network.
