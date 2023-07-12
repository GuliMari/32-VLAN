# 32-VLAN:
1. в Office1 в тестовой подсети появляется сервера с доп интерфесами и адресами в internal сети testLAN:
- testClient1 - 10.10.10.254
- testClient2 - 10.10.10.254
- testServer1- 10.10.10.1
- testServer2- 10.10.10.1
развести вланами testClient1 <-> testServer1 и testClient2 <-> testServer2
2. между centralRouter и inetRouter "пробросить" 2 линка (общая inernal сеть) и объединить их в бонд, проверить работу c отключением интерфейсов



## Выполнение:
1. Разворачиваем стенд и проверяем `vlan1`:
```bash
[root@testClient1 ~]# ping 10.10.10.254
PING 10.10.10.254 (10.10.10.254) 56(84) bytes of data.
64 bytes from 10.10.10.254: icmp_seq=1 ttl=64 time=0.017 ms
64 bytes from 10.10.10.254: icmp_seq=2 ttl=64 time=0.033 ms
64 bytes from 10.10.10.254: icmp_seq=3 ttl=64 time=0.034 ms
^C
```
Смотрим `vlan2`:
```bash
vagrant@testClient2:~$ ping 10.10.10.254
PING 10.10.10.254 (10.10.10.254) 56(84) bytes of data.
64 bytes from 10.10.10.254: icmp_seq=1 ttl=64 time=0.051 ms
64 bytes from 10.10.10.254: icmp_seq=2 ttl=64 time=0.025 ms
64 bytes from 10.10.10.254: icmp_seq=3 ttl=64 time=0.062 ms
^C
--- 10.10.10.254 ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 3057ms
rtt min/avg/max/mdev = 0.025/0.046/0.062/0.013 ms
```

2. Проверим `bond0`:
```bash
[root@inetRouter ~]# ping 192.168.255.2
PING 192.168.255.2 (192.168.255.2) 56(84) bytes of data.
64 bytes from 192.168.255.2: icmp_seq=1 ttl=64 time=0.463 ms
64 bytes from 192.168.255.2: icmp_seq=2 ttl=64 time=0.374 ms
^C
```
```bash
[root@centralRouter ~]# ip link set down eth1
```
```bash
[root@inetRouter ~]# ping 192.168.255.2
PING 192.168.255.2 (192.168.255.2) 56(84) bytes of data.
64 bytes from 192.168.255.2: icmp_seq=1 ttl=64 time=0.426 ms
64 bytes from 192.168.255.2: icmp_seq=2 ttl=64 time=0.352 ms
^C
```
Ping не пропал, трафик ушел на второй порт.
