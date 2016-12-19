### Create network interface on mac that does not have a physical NIC

- gist from [stackexchange-152331](http://unix.stackexchange.com/questions/152331/how-can-i-create-a-virtual-ethernet-interface-on-a-machine-without-a-physical-ad)


```bash
# Load a kernel module called dummy

$ sudo lsmod | grep dummy
$ sudo modprobe dummy
$ sudo lsmod | grep dummy
dummy                  12960  0

# Create a network interface

$ sudo ip link set name eth10 dev dummy0
  
# Verify
$ ip link show eth10
  6: eth10: <BROADCAST,NOARP> mtu 1500 qdisc noop state DOWN mode DEFAULT group default 
    link/ether c6:ad:af:42:80:45 brd ff:ff:ff:ff:ff:ff

# Optionally change the MAC address
$ sudo ifconfig eth10 hw ether 00:22:22:ff:ff:ff
$ ip link show eth10
  6: eth10: <BROADCAST,NOARP> mtu 1500 qdisc noop state DOWN mode DEFAULT group default 
    link/ether 00:22:22:ff:ff:ff brd ff:ff:ff:ff:ff:ff

# Create an alias
$ sudo ip addr add 192.168.100.199/24 brd + dev eth10 label eth10:0

# Verify the alias
$ ifconfig -a eth10
eth10: flags=130<BROADCAST,NOARP>  mtu 1500
        ether 00:22:22:ff:ff:ff  txqueuelen 0  (Ethernet)
        RX packets 0  bytes 0 (0.0 B)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 0  bytes 0 (0.0 B)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

$ ifconfig -a eth10:0
eth10:0: flags=130<BROADCAST,NOARP>  mtu 1500
        inet 192.168.100.199  netmask 255.255.255.0  broadcast 192.168.100.255
        ether 00:22:22:ff:ff:ff  txqueuelen 0  (Ethernet)

# Verify the alias : Aliter
$ ip a | grep -w inet
    inet 127.0.0.1/8 scope host lo
    inet 192.168.1.20/24 brd 192.168.1.255 scope global wlp3s0
    inet 192.168.122.1/24 brd 192.168.122.255 scope global virbr0
    inet 192.168.100.199/24 brd 192.168.100.255 scope global eth10:0

# Removing all of above
$ sudo ip addr del 192.168.100.199/24 brd + dev eth10 label eth10:0
$ sudo ip link delete eth10 type dummy
$ sudo rmmod dummy
```
