Чтобы проверить симметричность роутинга, добавляем на роутере R3 новую сеть на любой интерфейс, например, 
```
[root@R3 vagrant]# ip ad ad 192.168.50.1/24 dev eth2
```

После этого заходим на роутер R1 и смотрим таблицу маршрутизации: 
```
[vagrant@R1 ~]$ ip route
default via 10.0.2.2 dev eth0 proto dhcp metric 100
10.0.0.0/30 dev eth1 proto kernel scope link src 10.0.0.1 metric 101
10.0.0.0/24 dev eth1 proto kernel scope link src 10.0.0.1
10.0.2.0/24 dev eth0 proto kernel scope link src 10.0.2.15 metric 100
10.10.0.0/30 dev eth2 proto kernel scope link src 10.10.0.1 metric 102
10.10.0.0/24 dev eth2 proto kernel scope link src 10.10.0.1
10.20.0.0/30 via 10.0.0.2 dev eth1 proto ospf metric 20
10.20.0.0/24 via 10.0.0.2 dev eth1 proto ospf metric 20
192.168.50.0/24 proto ospf metric 20
        nexthop via 10.10.0.2 dev eth2 weight 1
        nexthop via 10.0.0.2 dev eth1 weight 1
```

Видим, что до сети 192.168.50.0/24 имеется два маршрута с одинаковым весом. 