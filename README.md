# TrueNASTorrentingStack
Use `macvlan`s to bind docker containers to a single address.

When torrenting we typically want to hide our real IP address so our ISP doesn't get mad at us.
[I have a VLAN, set up in pfsense (tutorial link)](https://boymoder.blog/thought?id=29), that routes through wireguard to an AirVPN VPN, thus hiding my
real ip address to anything connected to this network. This VLAN is set up as `br23` on my TrueNAS.
It uses the IP range `192.168.23.1/24`. 

Back in the TrueNAS Core days, we could easily route FreeBSD Jails through a single interface, in the web UI.
We could even use DHCP. TrueNAS Scale does away with Jails and uses docker instead.
In docker we can do something similar with `macvlan`s to allow our containers to get a new IP address routing
through a given network. Unfortunately we can't get DHCP addresses from our router, everything has to be set 
to static. 

Sadly, as of TrueNAS Scale Electric Eel, and apparently Fangtooth too, this is not possible in the web UI.
[Which has annoyed a lot of people.](https://forums.truenas.com/t/allow-apps-to-have-their-own-ip/12042)
This is how I have achieved it, using docker-compose.

*UPDATE*: As of TrueNAS 25, it is possible to limit which addresses docker containers bind to, but it is still not possible to configure containers' routing in the UI. We're still waiting for a `macvlan` wrapper.
