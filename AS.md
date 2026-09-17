# Autonomous Systems ( AS )

an autonomous system is like one **independently operated network**, the internet really just
connects the different AS. My ISP is an AS for example, it's free to operate how it wants for
the users connected to the ISP but needs to talk to other ISPs and such.

Think of AS link chunks of internet, clusters that are then again connected with each other.
Though they don't necessarily have to be large still they usually tend to be or have reasons
to be an AS because of the process.

## Who can be an AS?

Anyone that can get an ASN ( AS number ), these have to be globally unique. At the top level
IANA allocates blocks of ASN to regional intenret registries ( there are 5 ) who then regulate
and issue ASNs baed on conditions.

## Why would I want to get an AS?

So you either depend on one AS, usually your ISP OR don't in which case you need an AS.
Say my company uses ISP A but wants to have failover to ISP B, so it must not use rely
on any one of the ISPs and their AS, it would also then have some rules of preferential
ISP then in such a setup. Such indepenent decision making makes you "autonomous" ( in AS )
and basically requires you to get an ASN as such to enable this failover to different ISPs.

## So I got an ASN, how does anyone know me?

BGP - border gateway protocol is the communication layer; the border being the AS border.

### CIDR / Subnet notation

`203.0.113.0/24` the "/" describes how much of the prefix bits IDENTIFY the network.
So here 24 of the leading bits are for identification of the network itself, and the rest
belong to devices within the network. It's another way of saying, anything in that prefix is
mine.

### What can an AS advertise to own?

Generally subnets like above though if the prefix is full 32 it's IP addresses too.
Can be multiple.

### How does it advertise?

BGP sessions run between routers over TCP port 179.

Example, AS300 owns `203.0.113.0/24` and advertises this capability via BGP to
neighbor AS200 as via an `AS_PATH` for that subnet => `AS_PATH: 300`.

How does the very first AS get discovered? It has to have a neighbor to communicate right?
These are bootstrapped, bigger ISPs that that actually lay out the cables would have that
communication configured explicitly as pairs of ( neighbor IP, ASN ). The whole of BGP
is fundamentally a neighbor to neigbor protocol, all corporate owned.

## Malicious ASes

If you are big enough to get an AS, you can effectively get traffic routed your wayv ia BGP.

In 2008, Pakistan Telecom AS17557 wanting to block youtube, advertised youtube's subnet via
it's own ASN so as to block traffic to youtube. This leaked internationaly though through
a misconfiguration and caused international routing failures to youtube.

Other incidents too like in 2018 another AS advertised as Amazon's route 53 DNS and redirected
to fraud sites.

The way it's handled now is that if I'm the owner of an IP block I created signed ROA (Route
Origin Authorizations )saying for a prefix this ASN is the authorized origin, you
need to work with the same authorities that got you the ASN to get the subnet signed.
Then different network operators then can verify ( upto them still ) from central RPKI
repos ( RPKI = Resource Public Key Infrastructure ).
