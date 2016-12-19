### Add a new IP on an existing NIC adapter

- gist of [superuser-153415](http://superuser.com/questions/153415/how-do-you-create-a-new-network-eth)
- This can be done via ethernet alias

```bash
# Assumption: mac has got eth0 & eth1 nic adapters

# Create an alias with the required IP
$ sudo ifconfig eth0:0 192.168.1.6 up

# Persist reboot
$ cd /etc/sysconfig/network-scripts/
$ sudo cp ifcfg-eth0 ifcfg-eth0:0

# Edit this newly created file
DEVICE=eth0:0
IPADDR=192.168.1.6
NETMASK=255.255.255.0
NETWORK=192.168.1.0
ONBOOT=yes
NAME=eth0:0

# Remove or comment out any GATEWAY lines in both files,
# Add the GATEWAY line to your /etc/sysconfig/network file. 

# Restart
$ sudo ifup eth0:0 

# Or restart networking entirely
$ sudo service network restart

```
