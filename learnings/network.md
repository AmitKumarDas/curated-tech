#### References

- https://gitter.im/hashicorp-nomad/Lobby
  - a multi-host overlay or underlay network such as FanNetwork, Weave, Calico etc.

- FanNetwork

> Fan trades access to one **user-selected /8 (potentially external) address range** 
  for an expanded pool of “organisation-internal” addresses to be used by containers
  or virtual machines. Fan does this by mapping the addresses in a way that can be 
  computed, **rather than one that requires maintenance of distributed state** 
  (e.g., routing tables). 

- [the bits have hit fan](http://blog.dustinkirkland.com/2015/06/the-bits-have-hit-fan.html)

> ...is an extension of network tunnel driver in Linux kernel. Each container host has a fan 
  bridge that allows mapping of network traffic to any other container on the fan network.
  Overhead is nothing more than **IP-IP tunneling**. Quite simply, **a /16 network gets mapped 
  on onto an unused /8 network**, and **container traffic is routed by the host via an IP 
  tunnel**.
  
- IP tunnel

> IP in IP is an IP tunneling protocol that encapsulates one IP packet in another IP packet. 
  To encapsulate an IP packet in another IP packet, an outer header is added with SourceIP, 
  the entry point of the tunnel and the Destination point, the exit point of the tunnel. 
  While doing this, the inner packet is unmodified (except the TTL field, which is decremented).
  The Don't Fragment and the Type Of Service fields should be copied to the outer packet. 
  If the packet size is greater than the Path MTU, the packet is fragmented in the encapsulator, 
  as the outer header should be included. The decapsulator will reassemble the packet.

> TODO - https://www.flockport.com/simplify-container-networking-with-ubuntu-fan-project/
