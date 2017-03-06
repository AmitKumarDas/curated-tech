#### Snippets

- Restart if already running, else start

  ```bash
  initctl status etcd && initctl restart etcd || initctl start etcd
  ```
- Install if needed

  ```bash
  if [[ ! -e /usr/bin/curl ]]; then
    apt-get update
    apt-get -yqq install curl
  fi
  ```
- Add a new IP on an existing NIC adapter

  ```bash
  # gist of superuser-153415
  # This can be done via ethernet alias
  # Assumption: mac has got eth0 & eth1 nic adapters

  # Create an alias with the required IP
  $ sudo ifconfig eth0:0 192.168.1.6 up

  # To persist reboot
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
