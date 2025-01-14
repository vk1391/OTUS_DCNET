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
