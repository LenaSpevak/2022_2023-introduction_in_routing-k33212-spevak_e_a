#### University: [ITMO University](https://##3itmo.ru/ru/)
#### Faculty: [FICT](https://fict.itmo.ru)
#### Course: [Introduction in routing](https://github.com/itmo-ict-faculty/introduction-in-routing)
#### Year: 2022/2023
#### Group: K33212
#### Author: Spevak Elena Aleksandrovna 
#### Lab: Lab3
#### Date of create: 1.12.2022
#### Date of finished: 2.12.2022

# **Отчёт по лабораторной работе №3**

# Эмуляция распределенной корпоративной сети связи, настройка OSPF и MPLS, организация первого EoMPLS

# Цель работы
Изучить протоколы OSPF и MPLS, механизмы организации EoMPLS.

В ходе работы был подлючён офис в Нбю-Йорке в общей сети IP/MPLS и организовано EoMPLS между SGI-Prism и компьютером инженеров в офисе в Санкт-Петербурге.

Была создана  IP/MPLS сеть свзяи для "RogaIKopita Games" в ContainerLab. Топология описана в [yaml-файле](https://github.com/LenaSpevak/2022_2023-introduction_in_routing-k33212-spevak-e-a/blob/main/lab3/task3.yaml)

![Схема сети](https://github.com/LenaSpevak/2022_2023-introduction_in_routing-k33212-spevak-e-a/blob/main/lab3/lab3.drawio.png)

На роутерах были настроены IP адреса интерфесов, OSPF и MPLS.

Тексты конфигураций каждого из сетевых устройств:
- Роутер R01.NY:
```/interface bridge
add name=EoMPLS_B
add name=Lo0
/interface vpls
add cisco-style=yes cisco-style-id=100 disabled=no l2mtu=1500 mac-address=02:F6:E3:4B:B2:EB name=EoMPLS remote-peer=\
    4.4.4.4
/interface wireless security-profiles
set [ find default=yes ] supplicant-identity=MikroTik
/routing ospf instance
set [ find default=yes ] router-id=1.1.1.1
/interface bridge port
add bridge=EoMPLS_B interface=ether2
add bridge=EoMPLS_B interface=EoMPLS
/ip address
add address=172.31.255.30/30 interface=ether1 network=172.31.255.28
add address=172.13.1.1/30 interface=ether3 network=172.13.1.0
add address=172.13.2.1/30 interface=ether4 network=172.13.2.0
add address=1.1.1.1 interface=Lo0 network=1.1.1.1
/ip dhcp-client
add disabled=no interface=ether1
/mpls ldp
set enabled=yes transport-address=1.1.1.1
/mpls ldp interface
add interface=ether3
add interface=ether4
/routing ospf network
add area=backbone
/system identity
set name=R01.NY```

- Роутер R01.LND:
```R01.LND:
/interface bridge
add name=Lo0
/interface wireless security-profiles
set [ find default=yes ] supplicant-identity=MikroTik
/routing ospf instance
set [ find default=yes ] router-id=2.2.2.2
/ip address
add address=172.31.255.30/30 interface=ether1 network=172.31.255.28
add address=172.13.1.2/30 interface=ether2 network=172.13.1.0
add address=172.13.3.1/30 interface=ether3 network=172.13.3.0
add address=2.2.2.2 interface=Lo0 network=2.2.2.2
/ip dhcp-client
add disabled=no interface=ether1
/mpls ldp
set enabled=yes
/mpls ldp interface
add interface=ether2
add interface=ether3
/routing ospf network
add area=backbone
/system identity
set name=R01.LND```

- Роутер R01.HKI:
```/interface bridge
add name=Lo0
/interface wireless security-profiles
set [ find default=yes ] supplicant-identity=MikroTik
/routing ospf instance
set [ find default=yes ] router-id=3.3.3.3
/ip address
add address=172.31.255.30/30 interface=ether1 network=172.31.255.28
add address=172.13.3.2/30 interface=ether2 network=172.13.3.0
add address=172.13.4.1/30 interface=ether3 network=172.13.4.0
add address=172.13.5.1/30 interface=ether4 network=172.13.5.0
add address=3.3.3.3 interface=Lo0 network=3.3.3.3
/ip dhcp-client
add disabled=no interface=ether1
/mpls ldp
set enabled=yes
/mpls ldp interface
add interface=ether2
add interface=ether3
add interface=ether4
/routing ospf network
add area=backbone
/system identity
set name=R01.HKI:
/interface bridge
add name=Lo0
/interface wireless security-profiles
set [ find default=yes ] supplicant-identity=MikroTik
/routing ospf instance
set [ find default=yes ] router-id=3.3.3.3
/ip address
add address=172.31.255.30/30 interface=ether1 network=172.31.255.28
add address=172.13.3.2/30 interface=ether2 network=172.13.3.0
add address=172.13.4.1/30 interface=ether3 network=172.13.4.0
add address=172.13.5.1/30 interface=ether4 network=172.13.5.0
add address=3.3.3.3 interface=Lo0 network=3.3.3.3
/ip dhcp-client
add disabled=no interface=ether1
/mpls ldp
set enabled=yes
/mpls ldp interface
add interface=ether2
add interface=ether3
add interface=ether4
/routing ospf network
add area=backbone
/system identity
set name=R01.HKI```

- Роутер R01.SPB:
```/interface bridge
add name=EoMPLS_B
add name=Lo0
/interface vpls
add cisco-style=yes cisco-style-id=100 disabled=no l2mtu=1500 mac-address=02:46:B4:78:1F:E5 name=EoMPLS remote-peer=\
    1.1.1.1
/interface wireless security-profiles
set [ find default=yes ] supplicant-identity=MikroTik
/routing ospf instance
set [ find default=yes ] router-id=4.4.4.4
/interface bridge port
add bridge=EoMPLS_B interface=ether4
add bridge=EoMPLS_B interface=EoMPLS
/ip address
add address=172.31.255.30/30 interface=ether1 network=172.31.255.28
add address=172.13.5.2/30 interface=ether2 network=172.13.5.0
add address=172.13.6.1/30 interface=ether3 network=172.13.6.0
add address=4.4.4.4 interface=Lo0 network=4.4.4.4
/ip dhcp-client
add disabled=no interface=ether1
/mpls ldp
set enabled=yes
/mpls ldp interface
add interface=ether2
add interface=ether3
/routing ospf network
add area=backbone
/system identity
set name=R01.SPB```

- Роутер R01.MSK:
```/interface bridge
add name=Lo0
/interface wireless security-profiles
set [ find default=yes ] supplicant-identity=MikroTik
/routing ospf instance
set [ find default=yes ] router-id=5.5.5.5
/ip address
add address=172.31.255.30/30 interface=ether1 network=172.31.255.28
add address=172.13.6.2/30 interface=ether3 network=172.13.6.0
add address=172.13.7.1/30 interface=ether2 network=172.13.7.0
add address=5.5.5.5 interface=Lo0 network=5.5.5.5
/ip dhcp-client
add disabled=no interface=ether1
/mpls ldp
set enabled=yes
/mpls ldp interface
add interface=ether2
add interface=ether3
/routing ospf network
add area=backbone
/system identity
set name=R01.MSK```

- Роутер R01.LBN:
```/interface bridge
add name=Lo0
/interface wireless security-profiles
set [ find default=yes ] supplicant-identity=MikroTik
/routing ospf instance
set [ find default=yes ] router-id=6.6.6.6
/ip address
add address=172.31.255.30/30 interface=ether1 network=172.31.255.28
add address=172.13.2.2/30 interface=ether2 network=172.13.2.0
add address=172.13.4.2/30 interface=ether3 network=172.13.4.0
add address=172.13.7.2/30 interface=ether4 network=172.13.7.0
add address=6.6.6.6 interface=Lo0 network=6.6.6.6
/ip dhcp-client
add disabled=no interface=ether1
/mpls ldp
set enabled=yes
/mpls ldp interface
add interface=ether2
add interface=ether3
add interface=ether4
/routing ospf network
add area=backbone
/system identity
set name=R01.LBN```

На комьютерах были настроены IP адреса интерфейсов:
-SGI-Prism:
```/interface wireless security-profiles
set [ find default=yes ] supplicant-identity=MikroTik
/ip address
add address=172.31.255.30/30 interface=ether1 network=172.31.255.28
add address=10.0.0.1/24 interface=ether2 network=10.0.0.0
/ip dhcp-client
add disabled=no interface=ether1
/system identity
set name=SGI-Prism```

-PC1:
```/interface wireless security-profiles
set [ find default=yes ] supplicant-identity=MikroTik
/ip address
add address=172.31.255.30/30 interface=ether1 network=172.31.255.28
add address=10.0.0.2/24 interface=ether2 network=10.0.0.0
/ip dhcp-client
add disabled=no interface=ether1
/system identity
set name=PC1```

Таблицы MPLS маршрутов на роутерах:

![NY](https://github.com/LenaSpevak/2022_2023-introduction_in_routing-k33212-spevak-e-a/blob/main/lab3/mpls_table-NY.png)

![SPB](https://github.com/LenaSpevak/2022_2023-introduction_in_routing-k33212-spevak-e-a/blob/main/lab3/mpls_table-SPB.png)

![LND](https://github.com/LenaSpevak/2022_2023-introduction_in_routing-k33212-spevak-e-a/blob/main/lab3/mpls_table-LND.png)

![MSK](https://github.com/LenaSpevak/2022_2023-introduction_in_routing-k33212-spevak-e-a/blob/main/lab3/mpls_table-MSK.png)

![HKI](https://github.com/LenaSpevak/2022_2023-introduction_in_routing-k33212-spevak-e-a/blob/main/lab3/mpls_table-HLK.png)

С помощью EoMPLS были связаны порты eth2 и eth4 на роутерах R01.NY и R01.SPB соответсвенно. Через тунель 100 можно организовать соединение компьютеров.

Для проверки связанности офисов в Нью-Йорке и Санкт-Петербурге были пропингованы компьютеры:

![PingSGI-Prism](https://github.com/LenaSpevak/2022_2023-introduction_in_routing-k33212-spevak-e-a/blob/main/lab3/SGI-Prism_ping.png)

![PingPC!](https://github.com/LenaSpevak/2022_2023-introduction_in_routing-k33212-spevak-e-a/blob/main/lab3/PC1_ping.png)

**Вывод**
Были изучены протоколы OSPF, MPLS и механизмы организации EoMPLS. полученные знания были применены в построении сети связи для "RogaIKopita Games". 
