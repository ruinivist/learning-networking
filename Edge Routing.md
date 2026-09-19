# Edge Routing

Edge routing is basically getting a user to the closest / best Point of Presence (POP).

If I have servers in 10 locations, the question is: how do I automatically get the user to the one with the best latency?

## GeoDNS

aka one way to do Global Server Load Balancing (GSLB).

Each location has a different IP.

The intelligence lives in the authoritative DNS server, which looks at where the DNS request came from and returns the IP of an appropriate nearby region.

```text
India → Mumbai IP
Europe → Frankfurt IP
```

This is usually based on the resolver's location, so it doesn't necessarily represent the actual network distance or latency to the user.

## Anycast

With Anycast, multiple POPs advertise the **same IP prefix via BGP**.

```text
Mumbai    ─┐
Singapore ├─ advertise same prefix
Frankfurt ┘
```

The Internet sees multiple BGP routes to the same destination, and each network picks its preferred route.

So:

```text
India user  → Mumbai
Japan user  → Singapore
EU user     → Frankfurt
```

All of them can be connecting to the exact same IP.

Importantly, this is not broadcast. An ISP may know multiple routes in its BGP control plane, but it forwards packets using one selected route. The request does not eventually reach all POPs.

Also, the servers themselves don't need their own AS. The network/router advertising the prefix participates in BGP.

## What does an edge provider give me?

In theory, yes: if I have servers in multiple locations, IP space, BGP connectivity, and advertise the same prefix from all of them, the Internet will naturally steer traffic to different POPs.

Edge providers give you things like these which helps you operate it well and
have "someone" to ensure that you are not at whims of the network.

```text
global POPs
+ lots of ISP peering
+ BGP traffic engineering
+ health checks / route withdrawal
+ DDoS protection
+ caching / TLS / WAF
+ failover
```
