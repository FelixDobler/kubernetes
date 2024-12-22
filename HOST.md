need to set up wireguard to vserver
add routes on vserver
get ipv4 traffic working too

# vServer Routes
```
iptables -A FORWARD -i eth0 -o wg-k8s -d 10.9.0.0/16 -j ACCEPT
iptables -A FORWARD -i wg-k8s -o eth0 -s 10.9.0.0/16 -j ACCEPT

ip6tables -A FORWARD -i eth0 -o wg-k8s -d fd00:cafe:10::2 -j ACCEPT
ip6tables -A FORWARD -i wg-k8s -o eth0 -s fd00:cafe:10::/48 -j ACCEPT
ip6tables -t nat -A PREROUTING -d 2a03:4000:3e:20d:3410:ff:fe64:eec5 -j DNAT --to-destination fd00:cafe:10::2
ip6tables -t nat -A POSTROUTING -s fd00:cafe:10::2 -d ::/0 -j SNAT --to-source 2a03:4000:3e:20d:3410:ff:fe64:eec5
```


```python
Chain FORWARD (policy DROP 175 packets, 13971 bytes)
 pkts bytes target     prot opt in     out     source               destination
    0     0 ACCEPT     0    --  eth0   wg-k8s  ::/0                 2a03:4000:3e:20d:3410:ff:fe64:eec5
 9133  955K ACCEPT     0    --  wg-k8s *       ::/0                 ::/0
 9058 3399K ACCEPT     0    --  *      wg-k8s  ::/0                 ::/0
```

```
Chain PREROUTING (policy ACCEPT 4956 packets, 360K bytes)
 pkts bytes target     prot opt in     out     source               destination
  425 27252 DNAT       0    --  *      *       ::/0                 2a03:4000:3e:20d:3410:ff:fe64:eec5  to:fd00:cafe:10::2
Chain POSTROUTING (policy ACCEPT 42 packets, 3336 bytes)
 pkts bytes target     prot opt in     out     source               destination
 2093  183K MASQUERADE  0    --  *      eth0    ::/0                 ::/0
```


# Node Routes

```
Chain PREROUTING (policy ACCEPT 0 packets, 0 bytes)
 pkts bytes target     prot opt in     out     source               destination
  351 30915 KUBE-SERVICES  0    --  *      *       ::/0                 ::/0                 /* kubernetes service portals */
    2   144 DNAT       6    --  wg-vserver *       ::/0                 ::/0                 tcp dpt:80 to:fd00:cafe:43::10
```


# Redirect incoming HTTP traffic (new connections) on wg0 to the ingress-nginx service (IPv6 address)
ip6tables -t nat -A PREROUTING -i wg0 -p tcp --dport 80 -m conntrack --ctstate NEW -j DNAT --to-destination [2001:db8::1]:80

# Redirect incoming HTTPS traffic (new connections) on wg0 to the ingress-nginx service (IPv6 address)
ip6tables -t nat -A PREROUTING -i wg0 -p tcp --dport 443 -m conntrack --ctstate NEW -j DNAT --to-destination [2001:db8::1]:443
