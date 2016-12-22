#### bootcfg

> ...can be installed on any Linux distribution.

- [bootcfg group & profile](https://github.com/coreos/coreos-baremetal/blob/master/Documentation/bootcfg.md)
  - the config files participate in the order mentioned below:
    - group file to profile file to ignition file
    - metadata is defined at group file & used at ignition file
    - ignition file can be used for setting up systemd units
    - ignition file can be used to create complex scripting logic
      - e.g. [bootkube-controller.yaml](https://github.com/coreos/coreos-baremetal/blob/master/examples/ignition/bootkube-controller.yaml#L148)
  - profile points to a ignition file or a cloud-config file
  - profile can also point to generic config files (i.e. non-ignition or non-cloud-config file)
  - group consists of nodeX.json files
  - each nodeX.json will map a profile id against a mac address
    - this is done under selector tag
    - selector can accept *uuid, mac, hostname, serial* tags
  - A nodeX.json can also have metadata e.g. various ENV options
  - ignition configs are .json files.
    - ignition files can be generated from Fuze config i.e. .yaml file
  - bootcfg can serve static assets (*i.e. artifacts*) locally
    - e.g. profile can refer to these local artifact than downloading
  
- **Other Notes**

> ...bootcfg does not implement or exec a DHCP/TFTP server. Read network setup or use the 
  coreos/dnsmasq image if you need a quick DHCP, proxyDHCP, TFTP, or DNS setup.

> Ignition 2.0.0+ configs are versioned, machine-friendly JSON documents 
  (which contain encoded file contents). Operators should write and maintain 
  configs in a human-friendly format, such as CoreOS fuze configs.

> See [examples/ignition](https://github.com/coreos/coreos-baremetal/tree/master/examples/ignition)
  for numerous Fuze template examples.


#### References

- https://github.com/coreos/coreos-baremetal/
- https://github.com/coreos/container-linux-config-transpiler
- https://gist.github.com/jamesbuckett/c3a98276648eded4cc09a06d457f8f24
