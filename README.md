	Настройка протокола RIP

Ильющенко Анастасия
n = 11



	#1. Настроить ip-адреса, маски и default gateway на хостах и серверах:
PC0:
	IP address:			10.11.1.21
	Mask:				255.255.255.0
	default gateway:	10.11.1.2

PC1:
	IP address:			10.11.1.22
	Mask:				255.255.255.0
	default gateway:	10.11.1.2

Server1:
	IP address:			192.168.1.21
	Mask:				255.255.255.0
	default gateway:	192.168.1.1

Server2:
	IP address:			192.168.2.21
	Mask:				255.255.255.0
	default gateway:	192.168.2.1

Server3:
	IP address:			192.168.3.21
	Mask:				255.255.255.0
	default gateway:	192.168.3.1




	#2. Настроить роутеры
##Router3:

Router>enable
Router#conf t

Router(config)#int fa0/0 
Router(config-if)#ip address 10.11.1.2 255.255.255.0
Router(config-if)#no shutdown 
Router(config-if)#exit 

Router(config)#int fa0/1 
Router(config-if)#ip address 192.168.10.5 255.255.255.252 
Router(config-if)#no shutdown 
Router(config-if)#exit

Router(config)#int eth1/0 
Router(config-if)#ip address 192.168.3.1 255.255.255.0 
Router(config-if)#no shutdown 
Router(config-if)#exit

Router(config)#router rip
Router(config-router)#network 192.168.3.0
Router(config-router)#network 192.168.10.4
Router(config-router)#network 10.11.1.0
Router#show ip route
Codes: C - connected, S - static, I - IGRP, R - RIP, M - mobile, B - BGP
       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
       N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
       E1 - OSPF external type 1, E2 - OSPF external type 2, E - EGP
       i - IS-IS, L1 - IS-IS level-1, L2 - IS-IS level-2, ia - IS-IS inter area
       * - candidate default, U - per-user static route, o - ODR
       P - periodic downloaded static route

Gateway of last resort is not set

     10.0.0.0/24 is subnetted, 1 subnets
C       10.11.1.0 is directly connected, FastEthernet0/0
R    192.168.1.0/24 [120/1] via 10.11.1.1, 00:00:25, FastEthernet0/0
R    192.168.2.0/24 [120/1] via 192.168.10.6, 00:00:05, FastEthernet0/1
C    192.168.3.0/24 is directly connected, Ethernet1/0
     192.168.10.0/30 is subnetted, 2 subnets
R       192.168.10.0 [120/1] via 192.168.10.6, 00:00:05, FastEthernet0/1
C       192.168.10.4 is directly connected, FastEthernet0/1
Router#show ip interface brief
Interface              IP-Address      OK? Method Status                Protocol 
FastEthernet0/0        10.11.1.2       YES manual up                    up 
FastEthernet0/1        192.168.10.5    YES manual up                    up 
Ethernet1/0            192.168.3.1     YES manual up                    up 
Vlan1                  unassigned      YES unset  administratively down down




##Router2:

Router>enable
Router#conf t

Router(config)#int fa0/0 
Router(config-if)#ip address 192.168.10.2 255.255.255.252
Router(config-if)#no shutdown 
Router(config-if)#exit 

Router(config)#int fa0/1 
Router(config-if)#ip address 192.168.10.6 255.255.255.252 
Router(config-if)#no shutdown 
Router(config-if)#exit

Router(config)#int eth1/0 
Router(config-if)#ip address 192.168.2.1 255.255.255.0 
Router(config-if)#no shutdown 
Router(config-if)#exit

Router(config)#router rip
Router(config-router)#network 192.168.10.4
Router(config-router)#network 192.168.2.0
Router(config-router)#network 192.168.10.0
Router#show ip route
Codes: C - connected, S - static, I - IGRP, R - RIP, M - mobile, B - BGP
       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
       N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
       E1 - OSPF external type 1, E2 - OSPF external type 2, E - EGP
       i - IS-IS, L1 - IS-IS level-1, L2 - IS-IS level-2, ia - IS-IS inter area
       * - candidate default, U - per-user static route, o - ODR
       P - periodic downloaded static route

Gateway of last resort is not set

R    10.0.0.0/8 [120/1] via 192.168.10.5, 00:00:05, FastEthernet0/1
                [120/1] via 192.168.10.1, 00:00:10, FastEthernet0/0
R    192.168.1.0/24 [120/1] via 192.168.10.1, 00:00:10, FastEthernet0/0
C    192.168.2.0/24 is directly connected, Ethernet1/0
R    192.168.3.0/24 [120/1] via 192.168.10.5, 00:00:05, FastEthernet0/1
     192.168.10.0/30 is subnetted, 2 subnets
C       192.168.10.0 is directly connected, FastEthernet0/0
C       192.168.10.4 is directly connected, FastEthernet0/1

Router#show ip interface brief
Interface              IP-Address      OK? Method Status                Protocol 
FastEthernet0/0        192.168.10.2    YES manual up                    up 
FastEthernet0/1        192.168.10.6    YES manual up                    up 
Ethernet1/0            192.168.2.1     YES manual up                    up 
Vlan1                  unassigned      YES unset  administratively down down



##Router1:

Router>enable
Router#conf t

Router(config)#int fa0/0 
Router(config-if)#ip address 192.168.10.1 255.255.255.252
Router(config-if)#no shutdown 
Router(config-if)#exit 

Router(config)#int fa0/1 
Router(config-if)#ip address 10.11.1.1 255.255.255.0
Router(config-if)#no shutdown 
Router(config-if)#exit

Router(config)#int eth1/0 
Router(config-if)#ip address 192.168.1.1 255.255.255.0 
Router(config-if)#no shutdown 
Router(config-if)#exit

Router(config)#router rip
Router(config-router)#network 192.168.1.0
Router(config-router)#network 192.168.10.0
Router(config-router)#network 10.11.1.0
Router#show ip route
Codes: C - connected, S - static, I - IGRP, R - RIP, M - mobile, B - BGP
       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
       N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
       E1 - OSPF external type 1, E2 - OSPF external type 2, E - EGP
       i - IS-IS, L1 - IS-IS level-1, L2 - IS-IS level-2, ia - IS-IS inter area
       * - candidate default, U - per-user static route, o - ODR
       P - periodic downloaded static route

Gateway of last resort is not set

     10.0.0.0/24 is subnetted, 1 subnets
C       10.11.1.0 is directly connected, FastEthernet0/1
C    192.168.1.0/24 is directly connected, Ethernet1/0
R    192.168.2.0/24 [120/1] via 192.168.10.2, 00:00:23, FastEthernet0/0
R    192.168.3.0/24 [120/1] via 10.11.1.2, 00:00:25, FastEthernet0/1
     192.168.10.0/30 is subnetted, 2 subnets
C       192.168.10.0 is directly connected, FastEthernet0/0
R       192.168.10.4 [120/1] via 192.168.10.2, 00:00:23, FastEthernet0/0

Router#show ip interface brief
Interface              IP-Address      OK? Method Status                Protocol 
FastEthernet0/0        192.168.10.1    YES manual up                    up 
FastEthernet0/1        10.11.1.1       YES manual up                    up 
Ethernet1/0            192.168.1.1     YES manual up                    up 
Vlan1                  unassigned      YES unset  administratively down down
