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

the next steps were to implement L2 configurations.

I began with spanning tree. I configured rapid-pvst on each switch and set each dsw to be the root switch for two vlans: dsw7 root bridge for vlan 10 and 20 while dsw8 is the root switch for vlan 30 and 40. I then did the same with dsw9 and 10 for vlan 50 - 80. 

Once validated all switches are properly partaking in rapid-pvst and fully converged, I moved onto creating the etherchannels.

So i decided to go with the 802.3ad standard for port-channel. I configured the two links between each dsw to operate in trunk mode via int ran e0/2 - 3 -> sw tr encap dot1q -> sw mod tr. Then I ran int po1 to create a port-channel followed by the previous commands to configure the port-channel to operate in trunk mode as well. I then went back to int config mode for e0/2 & 3 and ran channel-group 1 mode active to have ports e0/2 & 3 to be members of port-channel 1 and operate in LACP (802.3ad) mode. 

Ive then validated via do sh etherchannel summary to ensure port-channel is L2 and up (SU), protocol is LACP, and the ports are e0/2 - 3.

L3 configurations:
first I ran do sh cdp neigh to view which ports are uplinks to csw. I then went into interface config and ran no sw to enable L3 routing on those switch ports. After that I assigned an ip address of 10.0.9x.1/30 to each link with x incrementing by +1. On the CSW i ran the same command but assigned the ip address of 10.0.9x.2/30. 

Next I created svis and assigned ip addresses to each vlan. The addressing scheme went 10.x.0.y/24 with x being equal to the vlan number and y being equal to the dsw number. EX: vlan 30 on dsw7 would be 10.30.0.(2 or 3)/24. 

HSRP config:
After creating svis, my next goal was to implement an FRHP protocol into each svi. I decided to use HSRP and configured DSW7 to be the active gateway for vlans 10 and 20 while dsw8 would be the active gateway for vlan 30 and 40. I've repeated this design on dsw9 and dsw10 with each being the active gateway for the first two vlans and the other for the remaining two. 

DSW7
standby 10 ip 10.10.0.1, standby 10 prio 110, standby 10 preempt
standby 20 ip 10.20.0.1, standby 20 prio 110, standby 20 preempt
standby 30 ip 10.30.0.1, standby 30 prio 90, standby 30 preempt
standby 40 ip 10.40.0.1, standby 40 prio 90, standby 40 preempt

DSW8
standby 10 ip 10.10.0.1, standby 10 prio 90, standby 10 preempt
standby 20 ip 10.20.0.1, standby 20 prio 90, standby 20 preempt
standby 30 ip 10.30.0.1, standby 30 prio 110, standby 30 preempt
standby 40 ip 10.40.0.1, standby 40 prio 110, standby 40 preempt
