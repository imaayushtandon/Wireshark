# Maximum Transmission Unit

The MTU is the maximum payload length for a particular transmission media. For example, the MTU for Ethernet is typically 1500 bytes. The maximum packet length for Ethernet is typically 1518 bytes, but that includes 14 bytes of Ethernet header and 4 bytes of CRC, leaving 1500 bytes of payload. When a VLAN tag is involved the packet length is increased to 1522 bytes. If a host wishes to send a packet larger than the MTU for a network, the packet must be broken up into chunks no larger than the MTU.

The smallest MTU between two hosts is known as the **path MTU**. This may be the same or smaller then the MTU of the interface the host sends the packet from. The reason for this is that the smallest MTU of *any* link between the routers in the path between both hosts determines the MTU of the whole path. 

In order to find the path MTU a host sends IP packets with the Don't Fragment flag set. When the packet size exceeds the path MTU size the router with the limiting MTU sends an ICMP packet back informing the sending host that the IP packet needs to be fragmented. The sending host now knows that the path MTU is limited below the packet sent. There are several RFC's on this process (For IPv4: [RFC 1191](https://tools.ietf.org/html/rfc1191), [RFC 4821](https://tools.ietf.org/html/rfc4821), [RFC 8899](https://tools.ietf.org/html/rfc8899) and IPv6: [RFC 8201](https://tools.ietf.org/html/rfc8201))

## MRU

Usually maximum transmission sizes of an interface are symetrical, i.e. the maximum transmission unit is equal to the maximum receive unit. But this is not by definition, but is a design feature of the interface.

## Common MTU Values

| Value | Link Type                                                                                        |
| ----- | ------------------------------------------------------------------------------------------------ |
| 65535 | PPP max, Hyperchannel                                                                            |
| 17914 | 16 Mbps Token Ring                                                                               |
| 4464  | 4 Mbps Token Ring                                                                                |
| 1500  | Ethernet, PPP default                                                                            |
| 1492  | IEEE 802.3/802.2                                                                                 |
| 1480  | PPPoE                                                                                            |
| 1460  | L2TP                                                                                             |
| 1454  | Optimal PPPoE-over-DSL MTU (see <http://www.mynetwatchman.com/kb/adsl/pppoemtu.htm> for details) |
| 1372  | PPTP (PPP/GRE/IP) default, Windows XP                                                            |
| 576   | X.25, default for many SLIP implementations                                                      |

## Setting MTU Values

### UNIX/Linux

To set MTU values, use the **ifconfig** command on many OSes.

``` 
 ifconfig <interface> mtu <value>
```

The **ifconfig** command might also report the MTU along with other information about the interface.

``` 
 ifconfig <interface>
```

On Linux with the **ip** command (from [iproute2](https://wiki.linuxfoundation.org/networking/iproute2)), to set the MTU:

```
  ip link set { DEVICE | group GROUP } mtu MTU
```

The **ip** command can show all details of the interface among which the MTU.

```
 ip link show [DEVICE]
```

### Windows

Getting your MTU value can be done using trial and error using the **ping** command.

    C:\>ping 10.1.1.1 -f -l 1373
    Pinging 10.1.1.1 with 1373 bytes of data:
    Packet needs to be fragmented but DF set.
    Packet needs to be fragmented but DF set.
    Packet needs to be fragmented but DF set.
    Packet needs to be fragmented but DF set.
    Ping statistics for 10.1.1.1:
        Packets: Sent = 4, Received = 0, Lost = 4 (100% loss),
    C:\>ping 10.1.1.1 -f -l 1372
    Pinging 10.1.1.1 with 1372 bytes of data:
    Reply from 10.1.1.1: bytes=1372 time=53ms TTL=254
    Reply from 10.1.1.1: bytes=1372 time=56ms TTL=254
    Reply from 10.1.1.1: bytes=1372 time=56ms TTL=254
    Reply from 10.1.1.1: bytes=1372 time=54ms TTL=254
    Ping statistics for 10.1.1.1:
        Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
    Approximate round trip times in milli-seconds:
        Minimum = 53ms, Maximum = 56ms, Average = 54ms

Setting your MTU value can be done using the registry. See KB article [158474](http://support.microsoft.com/default.aspx?scid=kb;en-us;158474) for more details.

See also [RFC 1191](https://tools.ietf.org/html/rfc1191#page-17) for a table of common MTUs.

---

Imported from https://wiki.wireshark.org/MTU on 2020-08-11 23:17:01 UTC
