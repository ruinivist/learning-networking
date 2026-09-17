# Network Address Translation ( NAT )

NAT is how bunch of devices on private network ( usually behind a router )
talk to internet while re-using the same public ip.

Think of this like a MANY private ip to a ONE public ip multiplexing.

"Basic" NAT itself is just "ip" translation based, so you need one to one mapping for a
private ip to a public ip for that. What is commonly used under the same
umbrella term is NAPT ( Network Address Port Translation ) or PAT ( Port Address
translation as vendors like Cisco name it ).

With NAPT, each device picks an ephemeral port ( aka port is allocated dynamically
as needed ) and then talks to the router with that port.

```
device -> my internet destination
192.168.1.10:50000 → 142.250.1.1:443

another one can use the same port as private ips differ
192.168.1.11:50000 → 142.250.1.1:443
```

What the router then does is in it's internal TRANSLATE them to different ports but a
common public ip

```
pub ip:60001
pub ip:60002
```

And stores the mapping of ( private ip, private port ) in its internal state table, the same
destination then in it's reply would then send the reply to the pub ip but with different ports,
so in the end there is no collision at all.

At the return path as well, the router will re-write the destination to the internal private ip,
and the private port from it's state table and move the packet along the private network.

### What about portless protocols?

Primarly NAPT is MADE to handle protocols like TCP and UDP which have ports. Though it has evolved
to handle the common portless protocols like ICMP which are portless, in which case routers
use an "id" field that's on ICMP packets but then ofc it NEEDS to understand ICMP and identify
ICMP.
