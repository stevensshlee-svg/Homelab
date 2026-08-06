Initial Configurations:
ASW, DSW, CSW: 
hostname A/D/CSW*
line con 0
exec-timeout 00
do wr

So I changed the hostname of each switch to ASW and their respective numbers first. I then went into console line configuration and set the inactivity timer to "00" which means never. These changes were made for better readability and easier configuration as I move between devices. In a real production environment best practice would be to set timers along with logon passwords.

Configuring VLANs
DSW7 = VTP Server
vtp domain: 3-tier_demo
vtp password: 3-tier_demo
vtp version 2

vlan 10 = IT , vlan 20 = HR , vlan 30 = Sales , vlan 40 = management

rest of switches vtp mode client

I configured this to make vlan creation across the network easy. In a production network enabling vtp poses a risk of vlans on a network being entirely deleted if a switch with a higher revision number connects. This is not ideal on a production network. 

link configurations: DSW e1/0 - 1 trunk
sw tr encap dot1q
sw mod tr

In order for vtp to share vlans the ports need to operate in trunk mode. Dot1q allows the switchport to tag the traffic coming in, therefore allowing the switchport to operate in trunk mode. 

then configured vlan 50, 60, 70 (IT HR Sales)
