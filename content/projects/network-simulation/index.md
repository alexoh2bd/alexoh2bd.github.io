---
title: "Network Simulation"
description: "Simulated the behavior of hosts sending packets over multiple routers"
---

## [Github link](https://github.com/alexoh2bd/CS_431-Network-Simulation)

Using a FreeBSD virtual machine, replicated the behavior of host computers sending packets through multiple networks and routers (stacks) Ethernet, IP, TCP, and ARP protocols. To accomplish this, the host and stack processes connect to [virtual switches](https://man.freebsd.org/cgi/man.cgi?query=vde_switch&sektion=1&apropos=0&manpath=FreeBSD+13.1-RELEASE+and+Ports).

{{< figure
    src="/assets/img/ss5.png"
    alt="Project screenshot"
    caption="Hosts and Stacks Demo"
>}}

## Features

* Hosts can send frames over multiple routers (stacks) to other hosts.
* Implements Ethernet, IP, TCP, and ARP stack protocols.
* Asserts IP packet header integrity: cross-verifies IP addresses in Routing Table, correct header checksums, length, protocol, and flags.
* Reroutes packets by building IP and Ethernet headers around packet at each router.
* Denies deviant frames/packets.
* Debugs with Wireshark
