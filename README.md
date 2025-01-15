# OTUS_DCNET

![alt-dtp](https://github.com/vk1391/OTUS_DCNET/blob/main/2025-01-13_16-16-03.png)

**Устройство** | **Интерфейс** | **ip address** | **Маска подсети**
 --- | --- | --- | -- 
 SPINE1 | lo0 | 1.1.1.1 | 255.255.255.255
 SPINE1 | lo1 | 101.1.1.1 | 255.255.255.255
 SPINE1 | eth1 | 10.1.11.1 | 255.255.255.252
 SPINE1 | eth2 | 10.1.12.1 | 255.255.255.252
 SPINE1 | eth3 | 10.1.13.1 | 255.255.255.252
 SPINE2 | lo0 | 2.2.2.2 | 255.255.255.255
 SPINE2 | lo1 | 101.2.2.2 | 255.255.255.255
 SPINE2 | eth1 | 10.2.11.1 | 255.255.255.252
 SPINE2 | eth2 | 10.2.12.1 | 255.255.255.252
 SPINE2 | eth3 | 10.2.13.1 | 255.255.255.252
 LEAF1 | lo0 | 11.11.11.11 | 255.255.255.255
 LEAF1 | lo1 | 101.11.11.11 | 255.255.255.255
 LEAF1 | eth1 | 10.1.11.2 | 255.255.255.252
 LEAF1 | eth2 | 10.2.11.2 | 255.255.255.252
 LEAF2 | lo0 | 12.12.12.12 | 255.255.255.255
 LEAF2 | lo1 | 101.12.12.12 | 255.255.255.252
 LEAF2 | eth1 | 10.1.12.2 | 255.255.255.252
 LEAF2 | eth2 | 10.2.12.2 | 255.255.255.252
 LEAF3 | lo0 | 13.13.13.13 | 255.255.255.255
 LEAF3 | lo1 | 101.13.13.13 | 255.255.255.255
 LEAF3 | eth1 | 10.1.13.2 | 255.255.255.252
 LEAF3 | eth2 | 10.2.13.2 | 255.255.255.252

# Построение Underlay сети(OSPF) 
## Задание:
 - Настроите OSPF в Underlay сети, для IP связанности между всеми сетевыми устройствами.
 - Зафиксируете в документации - план работы, адресное пространство, схему сети, конфигурацию устройств
 - Убедитесь в наличии IP связанности между устройствами в OSPF домене

![alt-dtp](https://github.com/vk1391/OTUS_DCNET/blob/main/ЦОД%202.png)

### Настроите OSPF в Underlay сети, для IP связанности между всеми сетевыми устройствами
- конфигурация ospf spine1:
```
ip routing
!
router ospf 1
   router-id 1.1.1.1
   bfd default
   area 0.0.0.1 stub
   network 1.1.1.1/32 area 0.0.0.1
   network 10.1.11.0/30 area 0.0.0.1
   network 10.1.12.0/30 area 0.0.0.1
   network 10.1.13.0/30 area 0.0.0.1
   network 101.1.1.1/32 area 0.0.0.1
   max-lsa 12000
```
- конфигурация ospf spine2:
```
ip routing
!
router ospf 1
   router-id 2.2.2.2
   bfd default
   area 0.0.0.1 stub
   network 2.2.2.2/32 area 0.0.0.1
   network 10.2.11.0/30 area 0.0.0.1
   network 10.2.12.0/30 area 0.0.0.1
   network 10.2.13.0/30 area 0.0.0.1
   network 101.2.2.2/32 area 0.0.0.1
   max-lsa 12000
```
- конфигурация ospf leaf1:
```
ip routing
!
router ospf 1
   router-id 11.11.11.11
   bfd default
   area 0.0.0.1 stub
   network 10.1.11.0/30 area 0.0.0.1
   network 10.2.11.0/30 area 0.0.0.1
   network 11.11.11.11/32 area 0.0.0.1
   network 101.11.11.11/32 area 0.0.0.1
   max-lsa 12000
```
- конфигурация ospf leaf2:
```
ip routing
!
router ospf 1
   router-id 12.12.12.12
   bfd default
   area 0.0.0.1 stub
   network 10.1.12.0/30 area 0.0.0.1
   network 10.2.12.0/30 area 0.0.0.1
   network 12.12.12.12/32 area 0.0.0.1
   network 101.12.12.12/32 area 0.0.0.1
   max-lsa 12000
```
- конфигурация ospf leaf3:
```
ip routing
!
router ospf 1
   router-id 13.13.13.13
   bfd default
   area 0.0.0.1 stub
   network 10.1.13.0/30 area 0.0.0.1
   network 10.2.13.0/30 area 0.0.0.1
   network 13.13.13.13/32 area 0.0.0.1
   network 101.13.13.13/32 area 0.0.0.1
   max-lsa 12000
```
### Убедитесь в наличии IP связанности между устройствами в OSPF домене
- таблица соседства spine1:
```
localhost#sh ip ospf neighbor 
Neighbor ID     Instance VRF      Pri State                  Dead Time   Address         Interface
12.12.12.12     1        default  1   FULL/BDR               00:00:35    10.1.12.2       Ethernet2
13.13.13.13     1        default  1   FULL/BDR               00:00:35    10.1.13.2       Ethernet3
11.11.11.11     1        default  1   FULL/DR                00:00:29    10.1.11.2       Ethernet1
```
- таблица соседства spine2:
```
localhost(config-router-ospf)#sh ip ospf nei
Neighbor ID     Instance VRF      Pri State                  Dead Time   Address         Interface
12.12.12.12     1        default  1   FULL/DR                00:00:32    10.2.12.2       Ethernet2
13.13.13.13     1        default  1   FULL/DR                00:00:34    10.2.13.2       Ethernet3
11.11.11.11     1        default  1   FULL/DR                00:00:34    10.2.11.2       Ethernet1
```
- таблица маршрутизации spine1:
```
Gateway of last resort is not set

 C        1.1.1.1/32 is directly connected, Loopback0
 O        2.2.2.2/32 [110/30] via 10.1.11.2, Ethernet1
                              via 10.1.12.2, Ethernet2
                              via 10.1.13.2, Ethernet3
 C        10.1.11.0/30 is directly connected, Ethernet1
 C        10.1.12.0/30 is directly connected, Ethernet2
 C        10.1.13.0/30 is directly connected, Ethernet3
 O        10.2.11.0/30 [110/20] via 10.1.11.2, Ethernet1
 O        10.2.12.0/30 [110/20] via 10.1.12.2, Ethernet2
 O        10.2.13.0/30 [110/20] via 10.1.13.2, Ethernet3
 O        11.11.11.11/32 [110/20] via 10.1.11.2, Ethernet1
 O        12.12.12.12/32 [110/20] via 10.1.12.2, Ethernet2
 O        13.13.13.13/32 [110/20] via 10.1.13.2, Ethernet3
 C        101.1.1.1/32 is directly connected, Loopback1
 O        101.2.2.2/32 [110/30] via 10.1.11.2, Ethernet1
                                via 10.1.12.2, Ethernet2
                                via 10.1.13.2, Ethernet3
 O        101.11.11.11/32 [110/20] via 10.1.11.2, Ethernet1
 O        101.12.12.12/32 [110/20] via 10.1.12.2, Ethernet2
 O        101.13.13.13/32 [110/20] via 10.1.13.2, Ethernet3
```
- таблица маршрутизации spine2:
```
Gateway of last resort is not set

 O        1.1.1.1/32 [110/30] via 10.2.11.2, Ethernet1
                              via 10.2.12.2, Ethernet2
                              via 10.2.13.2, Ethernet3
 C        2.2.2.2/32 is directly connected, Loopback0
 O        10.1.11.0/30 [110/20] via 10.2.11.2, Ethernet1
 O        10.1.12.0/30 [110/20] via 10.2.12.2, Ethernet2
 O        10.1.13.0/30 [110/20] via 10.2.13.2, Ethernet3
 C        10.2.11.0/30 is directly connected, Ethernet1
 C        10.2.12.0/30 is directly connected, Ethernet2
 C        10.2.13.0/30 is directly connected, Ethernet3
 O        11.11.11.11/32 [110/20] via 10.2.11.2, Ethernet1
 O        12.12.12.12/32 [110/20] via 10.2.12.2, Ethernet2
 O        13.13.13.13/32 [110/20] via 10.2.13.2, Ethernet3
 O        101.1.1.1/32 [110/30] via 10.2.11.2, Ethernet1
                                via 10.2.12.2, Ethernet2
                                via 10.2.13.2, Ethernet3
 C        101.2.2.2/32 is directly connected, Loopback1
 O        101.11.11.11/32 [110/20] via 10.2.11.2, Ethernet1
 O        101.12.12.12/32 [110/20] via 10.2.12.2, Ethernet2
 O        101.13.13.13/32 [110/20] via 10.2.13.2, Ethernet3
```
 - ping lo0 spine1 - lo0 spine2:
```
localhost#ping 2.2.2.2
PING 2.2.2.2 (2.2.2.2) 72(100) bytes of data.
80 bytes from 2.2.2.2: icmp_seq=1 ttl=63 time=21.6 ms
80 bytes from 2.2.2.2: icmp_seq=2 ttl=63 time=17.3 ms
80 bytes from 2.2.2.2: icmp_seq=3 ttl=63 time=17.3 ms
80 bytes from 2.2.2.2: icmp_seq=4 ttl=63 time=14.4 ms
80 bytes from 2.2.2.2: icmp_seq=5 ttl=63 time=16.4 ms

--- 2.2.2.2 ping statistics ---
5 packets transmitted, 5 received, 0% packet loss, time 73ms
rtt min/avg/max/mdev = 14.478/17.444/21.683/2.361 ms, pipe 2, ipg/ewma 18.262/19.454 ms
localhost#traceroute 2.2.2.2
traceroute to 2.2.2.2 (2.2.2.2), 30 hops max, 60 byte packets
 1  10.1.11.2 (10.1.11.2)  21.543 ms  29.202 ms  30.489 ms
 2  2.2.2.2 (2.2.2.2)  61.298 ms  62.389 ms  73.012 ms
```

# Построение Underlay сети(ISIS) 
## Задание:
- Настроите ISIS в Underlay сети, для IP связанности между всеми сетевыми устройствами.
- Зафиксируете в документации - план работы, адресное пространство, схему сети, конфигурацию устройств
- Убедитесь в наличии IP связанности между устройствами в ISIS домене
![alt-dtp](https://github.com/vk1391/OTUS_DCNET/blob/main/isis.jpg)
###  Настроите ISIS в Underlay сети, для IP связанности между всеми сетевыми устройствами.
- конфигурация isis spine1:
```
localhost#sh run | sec isis
interface Ethernet1
   isis enable s1
   isis circuit-type level-2
   isis network point-to-point
interface Ethernet2
   isis enable s1
   isis circuit-type level-2
   isis network point-to-point
interface Ethernet3
   isis enable s1
   isis circuit-type level-2
   isis network point-to-point
interface Loopback0
   isis enable s1
interface Loopback1
   isis enable s1
router isis s1
   net 49.0001.0001.0001.0001.00
   is-hostname s1
   router-id ipv4 1.1.1.1
   !
   address-family ipv4 unicast
      bfd all-interfaces
```
- конфигурация isis spine2:
```
localhost(config-router-isis)#sh run | sec isis
interface Ethernet1
   isis enable s2
   isis circuit-type level-2
   isis network point-to-point
interface Ethernet2
   isis enable s2
   isis circuit-type level-2
   isis network point-to-point
interface Ethernet3
   isis enable s2
   isis circuit-type level-2
   isis network point-to-point
interface Loopback0
   isis enable s2
interface Loopback1
   isis enable s2
router isis s2
   net 49.0002.0000.0000.0000.0002.00
   is-hostname s2
   !
   address-family ipv4 unicast
      bfd all-interfaces
```
- конфигурация isis leaf1:
```
localhost(config-router-isis)#sh run | sec isis
interface Ethernet1
   isis enable l1
   isis circuit-type level-2
   isis network point-to-point
interface Ethernet2
   isis enable l1
   isis circuit-type level-2
   isis network point-to-point
interface Loopback0
   isis enable l1
interface Loopback1
   isis enable l1
router isis l1
   net 49.0022.0022.0022.0022.00
   is-hostname l1
   router-id ipv4 11.11.11.11
   is-type level-2
   !
   address-family ipv4 unicast
      bfd all-interfaces
```
- конфигурация isis leaf2:
```
localhost(config-router-isis)#sh run | sec isis
interface Ethernet1
   isis enable l2
   isis circuit-type level-2
   isis network point-to-point
interface Ethernet2
   isis enable l2
   isis circuit-type level-2
   isis network point-to-point
interface Loopback0
   isis enable l2
interface Loopback1
   isis enable l2
router isis l2
   net 49.0022.0000.0000.0000.0022.00
   is-hostname l2
   is-type level-2
   !
   address-family ipv4 unicast
      bfd all-interfaces
```
- конфигурация isis leaf3:
```
localhost(config-router-isis)#sh run | sec isis
interface Ethernet1
   isis enable l3
   isis circuit-type level-2
   isis network point-to-point
interface Ethernet2
   isis enable l3
   isis circuit-type level-2
   isis network point-to-point
interface Loopback0
   isis enable l3
interface Loopback1
   isis enable l3
router isis l3
   net 49.0033.0000.0000.0000.0033.00
   is-hostname l3
   is-type level-2
   !
   address-family ipv4 unicast
```
### Убедитесь в наличии IP связанности между устройствами в ISIS домене
- таблица соседства spine1:
```
localhost#sh isis neighbors 
 
Instance  VRF      System Id        Type Interface          SNPA              State Hold time   Circuit Id          
s1        default  l2               L2   Ethernet2          P2P               UP    24          0D                  
s1        default  l3               L2   Ethernet3          P2P               UP    29          0D                  
s1        default  l1               L2   Ethernet1          P2P               UP    26          0F
```
- таблица соседства spine2:
```
localhost#sh isis neighbors 
 
Instance  VRF      System Id        Type Interface          SNPA              State Hold time   Circuit Id          
s2        default  l2               L2   Ethernet2          P2P               UP    29          0E                  
s2        default  l3               L2   Ethernet3          P2P               UP    29          0E                  
s2        default  l1               L2   Ethernet1          P2P               UP    26          10
```
- таблица маршрутизации spine1:
```
Gateway of last resort is not set

 C        1.1.1.1/32 is directly connected, Loopback0
 I L2     2.2.2.2/32 [115/30] via 10.1.11.2, Ethernet1
                              via 10.1.12.2, Ethernet2
                              via 10.1.13.2, Ethernet3
 C        10.1.11.0/30 is directly connected, Ethernet1
 C        10.1.12.0/30 is directly connected, Ethernet2
 C        10.1.13.0/30 is directly connected, Ethernet3
 I L2     10.2.11.0/30 [115/20] via 10.1.11.2, Ethernet1
 I L2     10.2.12.0/30 [115/20] via 10.1.12.2, Ethernet2
 I L2     10.2.13.0/30 [115/20] via 10.1.13.2, Ethernet3
 I L2     11.11.11.11/32 [115/20] via 10.1.11.2, Ethernet1
 I L2     12.12.12.12/32 [115/20] via 10.1.12.2, Ethernet2
 I L2     13.13.13.13/32 [115/20] via 10.1.13.2, Ethernet3
 C        101.1.1.1/32 is directly connected, Loopback1
 I L2     101.2.2.2/32 [115/30] via 10.1.11.2, Ethernet1
                                via 10.1.12.2, Ethernet2
                                via 10.1.13.2, Ethernet3
 I L2     101.11.11.11/32 [115/20] via 10.1.11.2, Ethernet1
 I L2     101.12.12.12/32 [115/20] via 10.1.12.2, Ethernet2
 I L2     101.13.13.13/32 [115/20] via 10.1.13.2, Ethernet3
```
- таблица маршрутизации spine2:
```
Gateway of last resort is not set

 I L2     1.1.1.1/32 [115/30] via 10.2.11.2, Ethernet1
                              via 10.2.12.2, Ethernet2
                              via 10.2.13.2, Ethernet3
 C        2.2.2.2/32 is directly connected, Loopback0
 I L2     10.1.11.0/30 [115/20] via 10.2.11.2, Ethernet1
 I L2     10.1.12.0/30 [115/20] via 10.2.12.2, Ethernet2
 I L2     10.1.13.0/30 [115/20] via 10.2.13.2, Ethernet3
 C        10.2.11.0/30 is directly connected, Ethernet1
 C        10.2.12.0/30 is directly connected, Ethernet2
 C        10.2.13.0/30 is directly connected, Ethernet3
 I L2     11.11.11.11/32 [115/20] via 10.2.11.2, Ethernet1
 I L2     12.12.12.12/32 [115/20] via 10.2.12.2, Ethernet2
 I L2     13.13.13.13/32 [115/20] via 10.2.13.2, Ethernet3
 I L2     101.1.1.1/32 [115/30] via 10.2.11.2, Ethernet1
                                via 10.2.12.2, Ethernet2
                                via 10.2.13.2, Ethernet3
 C        101.2.2.2/32 is directly connected, Loopback1
 I L2     101.11.11.11/32 [115/20] via 10.2.11.2, Ethernet1
 I L2     101.12.12.12/32 [115/20] via 10.2.12.2, Ethernet2
 I L2     101.13.13.13/32 [115/20] via 10.2.13.2, Ethernet3
```
- ping lo0 spine1 - lo0 spine2:
```
localhost#ping 2.2.2.2
PING 2.2.2.2 (2.2.2.2) 72(100) bytes of data.
80 bytes from 2.2.2.2: icmp_seq=1 ttl=63 time=22.7 ms
80 bytes from 2.2.2.2: icmp_seq=2 ttl=63 time=18.1 ms
80 bytes from 2.2.2.2: icmp_seq=3 ttl=63 time=15.9 ms
80 bytes from 2.2.2.2: icmp_seq=4 ttl=63 time=14.6 ms
80 bytes from 2.2.2.2: icmp_seq=5 ttl=63 time=17.8 ms

--- 2.2.2.2 ping statistics ---
5 packets transmitted, 5 received, 0% packet loss, time 78ms
rtt min/avg/max/mdev = 14.648/17.874/22.735/2.749 ms, pipe 2, ipg/ewma 19.731/20.212 ms
localhost#traceroute 2.2.2.2
traceroute to 2.2.2.2 (2.2.2.2), 30 hops max, 60 byte packets
 1  10.1.11.2 (10.1.11.2)  15.962 ms  26.506 ms  26.858 ms
 2  2.2.2.2 (2.2.2.2)  47.047 ms  51.166 ms  58.808 ms
```
# Построение Underlay сети(eBGP)
## Задание
 - Настроите BGP в Underlay сети, для IP связанности между всеми сетевыми устройствами. iBGP или eBGP - решать вам!
 - Зафиксируете в документации - план работы, адресное пространство, схему сети, конфигурацию устройств
 - Убедитесь в наличии IP связанности между устройствами в BGP домене
![alt-dtp](https://github.com/vk1391/OTUS_DCNET/blob/main/bgp.png)
### Настроите iBGP в Underlay сети, для IP связанности между всеми сетевыми устройствами
- конфигурация ibgp spine1:
```
localhost#sh run | sec bgp
router bgp 101
   router-id 1.1.1.1
   timers bgp 3 9
   neighbor underlay peer group
   neighbor underlay remote-as 101
   neighbor underlay next-hop-self
   neighbor underlay bfd
   neighbor underlay route-reflector-client
   neighbor 10.1.11.2 peer group underlay
   neighbor 10.1.12.2 peer group underlay
   neighbor 10.1.13.2 peer group underlay
   redistribute connected route-map redist
localhost(config-router-bgp)#sh run | sec route-map
route-map redist permit 10
   match interface Loopback0
```
- конфигурация ibgp spine2:
```
localhost(config-router-bgp)#sh run | sec bgp
router bgp 101
   router-id 2.2.2.2
   timers bgp 3 9
   neighbor underlay peer group
   neighbor underlay remote-as 101
   neighbor underlay next-hop-self
   neighbor underlay bfd
   neighbor 10.2.11.2 peer group underlay
   neighbor 10.2.12.2 peer group underlay
   neighbor 10.2.13.2 peer group underlay
   redistribute connected route-map redist
localhost(config-router-bgp)#sh run | sec route-map
route-map redist permit 10
   match interface Loopback0
```
- конфигурация ibgp leaf1:
```
localhost(config-router-bgp)#sh run | sec bgp
router bgp 101
   router-id 11.11.11.11
   timers bgp 3 9
   maximum-paths 2
   neighbor 10.1.11.1 remote-as 101
   neighbor 10.1.11.1 next-hop-self
   neighbor 10.1.11.1 bfd
   neighbor 10.2.11.1 remote-as 101
   neighbor 10.2.11.1 next-hop-self
   neighbor 10.2.11.1 bfd
   redistribute connected route-map redist
localhost(config-router-bgp)#sh run | sec route-map
route-map redist permit 10
   match interface Loopback0
```
- конфигурация ibgp leaf2:
```
localhost(config-router-bgp)#sh run | sec bgp
router bgp 101
   router-id 12.12.12.12
   timers bgp 3 9
   maximum-paths 2
   neighbor 10.1.12.1 remote-as 101
   neighbor 10.1.12.1 bfd
   neighbor 10.2.12.1 remote-as 101
   neighbor 10.2.12.1 bfd
   redistribute connected route-map redist
localhost(config-router-bgp)#sh run | sec route-map
route-map redist permit 10
   match interface Loopback0
router bgp 101
   redistribute connected route-map redist
- конфигурация ibgp leaf2:
```
- конфигурация ibgp leaf3:
```
localhost(config-router-bgp)#sh run | sec bgp
router bgp 101
   router-id 13.13.13.13
   timers bgp 3 9
   maximum-paths 2
   neighbor 10.1.13.1 remote-as 101
   neighbor 10.1.13.1 bfd
   neighbor 10.2.13.1 remote-as 101
   neighbor 10.2.13.1 bfd
   redistribute connected route-map redist
localhost(config-router-bgp)#sh run | sec route-map
route-map redist permit 10
   match interface Loopback0
```
### Убедитесь в наличии IP связанности между устройствами в BGP домене
- таблица соседства spine1:
```
localhost#sh ip bgp sum
BGP summary information for VRF default
Router identifier 1.1.1.1, local AS number 101
Neighbor Status Codes: m - Under maintenance
  Neighbor         V  AS           MsgRcvd   MsgSent  InQ OutQ  Up/Down State   PfxRcd PfxAcc
  10.1.11.2        4  101              573       587    0    0 00:22:22 Estab   1      1
  10.1.12.2        4  101              575       593    0    0 00:22:22 Estab   1      1
  10.1.13.2        4  101              570       589    0    0 00:22:22 Estab   1      1
```
- таблица соседства spine2:
```
localhost#sh ip bgp sum
BGP summary information for VRF default
Router identifier 2.2.2.2, local AS number 101
Neighbor Status Codes: m - Under maintenance
  Neighbor         V  AS           MsgRcvd   MsgSent  InQ OutQ  Up/Down State   PfxRcd PfxAcc
  10.2.11.2        4  101              589       591    0    0 00:23:01 Estab   1      1
  10.2.12.2        4  101              592       595    0    0 00:23:01 Estab   1      1
  10.2.13.2        4  101              587       591    0    0 00:23:01 Estab   1      1
```
- таблица маршрутизации leaf1:
```
 B I      1.1.1.1/32 [200/0] via 10.1.11.1, Ethernet1
 B I      2.2.2.2/32 [200/0] via 10.2.11.1, Ethernet2
 C        10.1.11.0/30 is directly connected, Ethernet1
 C        10.2.11.0/30 is directly connected, Ethernet2
 C        11.11.11.11/32 is directly connected, Loopback0
 B I      12.12.12.12/32 [200/0] via 10.1.11.1, Ethernet1
 B I      13.13.13.13/32 [200/0] via 10.1.11.1, Ethernet1
 C        101.11.11.11/32 is directly connected, Loopback1
```
- таблица маршрутизации leaf2:
```
 B I      1.1.1.1/32 [200/0] via 10.1.12.1, Ethernet1
 B I      2.2.2.2/32 [200/0] via 10.2.12.1, Ethernet2
 C        10.1.12.0/30 is directly connected, Ethernet1
 C        10.2.12.0/30 is directly connected, Ethernet2
 B I      11.11.11.11/32 [200/0] via 10.1.12.1, Ethernet1
 C        12.12.12.12/32 is directly connected, Loopback0
 B I      13.13.13.13/32 [200/0] via 10.1.12.1, Ethernet1
 C        101.12.12.12/32 is directly connected, Loopback1
```
- таблица маршрутизации leaf3:
```
 B I      1.1.1.1/32 [200/0] via 10.1.13.1, Ethernet1
 B I      2.2.2.2/32 [200/0] via 10.2.13.1, Ethernet2
 C        10.1.13.0/30 is directly connected, Ethernet1
 C        10.2.13.0/30 is directly connected, Ethernet2
 B I      11.11.11.11/32 [200/0] via 10.1.13.1, Ethernet1
 B I      12.12.12.12/32 [200/0] via 10.1.13.1, Ethernet1
 C        13.13.13.13/32 is directly connected, Loopback0
 C        101.13.13.13/32 is directly connected, Loopback1
```
- ping lo0 leaf1 - lo0 leaf3
```
localhost#ping 13.13.13.13 source 11.11.11.11
PING 13.13.13.13 (13.13.13.13) from 11.11.11.11 : 72(100) bytes of data.
80 bytes from 13.13.13.13: icmp_seq=1 ttl=63 time=24.0 ms
80 bytes from 13.13.13.13: icmp_seq=2 ttl=63 time=19.8 ms
80 bytes from 13.13.13.13: icmp_seq=3 ttl=63 time=15.5 ms
80 bytes from 13.13.13.13: icmp_seq=4 ttl=63 time=13.1 ms
80 bytes from 13.13.13.13: icmp_seq=5 ttl=63 time=16.5 ms

--- 13.13.13.13 ping statistics ---
5 packets transmitted, 5 received, 0% packet loss, time 83ms
rtt min/avg/max/mdev = 13.171/17.833/24.070/3.791 ms, pipe 2, ipg/ewma 20.813/20.766 ms
localhost#traceroute 13.13.13.13 ?
  interface  specify egress interface
  source     specify source address or choose an address from specified
             interface
  <cr>       

localhost#traceroute 13.13.13.13 source 11.11.11.11
traceroute to 13.13.13.13 (13.13.13.13), 30 hops max, 60 byte packets
 1  10.1.11.1 (10.1.11.1)  16.572 ms  24.671 ms  27.995 ms
 2  13.13.13.13 (13.13.13.13)  37.553 ms  42.440 ms  51.917 ms
```
