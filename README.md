# vyatta-sixxs

This is another approach then e.g. [vyatta-aiccu](https://github.com/fgp/vyatta-aiccu) to support AICCU and especially SixXS support for Vyatta/EdgeOS. Maybe a more fragile one.

It requires that the AICCU interface is setup outside of and before Vyatta (e.g. using Debian */etc/network/interfaces*) since this module relies on it's presence and that AICCU is started and controlled outside of Vyatta (e.g. simply using sysvinit). There are some special options needed for AICCU to function with a precreated tun interface.

The major task of this Vyatta module is to **apply the Vyatta firewall rules** onto an interface managed mainly outside Vyatta. Additional capabilities that are not tested but should work out of the box: description, traffic-policy, redirect.

## Sample

*/etc/network/interfaces*:

```
auto sixxs0
iface sixxs0 inet6 static
    address YOUR_IPv6
    netmask 64
    gateway POP_IPv6
    pre-up ip tuntap add mode tun $IFACE
    up ip link set mtu 1280 dev $IFACE
```

*/etc/aiccu.conf*:

```
username XXX
password XXX
ipv6_interface sixxs0
tunnel_id XXX
verbose false
daemonize true
automatic true
requiretls false
defaultroute true
noconfigure true
```

Note: some options may be redundant. The `ipv6_interface` name should match your precreated one and `noconfigure` should be `true` in our case.
