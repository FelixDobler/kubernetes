# 
Enable forwarding
```
echo "net.ipv4.ip_forward=1" >> /etc/sysctl.conf
sysctl -p
```

# Networking Config
```
# ip rule
from 10.44.0.0/24 lookup 200
from 10.44.1.0/24 lookup 200
```
```
root@futro:~# ip route
default via 192.168.1.1 dev vmbr0 proto kernel onlink
10.44.0.0/24 dev wg-pve-k8s-c scope link
10.44.1.0/24 dev vmbr1 proto kernel scope link src 10.44.1.1
10.44.1.1 via 10.44.0.1 dev wg-pve-k8s-c
192.168.1.0/24 dev vmbr0 proto kernel scope link src 192.168.1.150
```
```
# ip route show table 200
default via 10.44.0.1 dev wg-pve-k8s-c
```
```
# ip link
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN mode DEFAULT group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
2: nic0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel master vmbr0 state UP mode DEFAULT group default qlen 1000
    link/ether 4c:52:62:ba:a9:bc brd ff:ff:ff:ff:ff:ff
    altname enp2s0
    altname enx4c5262baa9bc
7: vmbr0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP mode DEFAULT group default qlen 1000
    link/ether 4c:52:62:ba:a9:bc brd ff:ff:ff:ff:ff:ff
8: vmbr1: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP mode DEFAULT group default qlen 1000
    link/ether 1e:96:62:43:4e:1e brd ff:ff:ff:ff:ff:ff
10: wg-pve-k8s-c: <POINTOPOINT,NOARP,UP,LOWER_UP> mtu 1420 qdisc noqueue state UNKNOWN mode DEFAULT group default qlen 1000
    link/none
11: tap100i0: <BROADCAST,MULTICAST,PROMISC,UP,LOWER_UP> mtu 1500 qdisc fq_codel master vmbr1 state UNKNOWN mode DEFAULT group default qlen 1000
    link/ether 1e:96:62:43:4e:1e brd ff:ff:ff:ff:ff:ff
```




# Access local network
```
root@futro:~# ip rule add from 10.44.1.0/24 to 192.168.1.0/24 lookup main
```

Add route 10.44.1.0/24 gw 192.168.1.150 on router
