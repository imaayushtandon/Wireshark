# Nmap Packet Capture (Npcap)

[Npcap](https://npcap.com/) is the Windows version of the [libpcap](http://www.tcpdump.org/) library; it includes a driver to support capturing packets.

Wireshark uses this library to capture live network data on Windows.

General information and downloads are available on the [Npcap web site](https://npcap.com/).

The libpcap file format description can be found at: [Development/LibpcapFileFormat](/Development/LibpcapFileFormat)

[Npcap 1.00 [2020-09-25]](https://github.com/nmap/npcap/blob/master/CHANGELOG.md#npcap-100-2020-09-25)  
"After more than 7 years of development and 170 previous public releases, the Nmap Project is delighted to release Npcap version 1.00!"

Npcap is the Nmap Project's packet sniffing library for Windows. It is based on the wonderful Winpcap / Libpcap libraries, but with improved improved speed, portability, security, and efficiency. In particular, Npcap offers:

  - NDIS 6 Support

  - [WinPcap](/WinPcap) compatibility

  - Loopback Packet Capture

---

Imported from https://wiki.wireshark.org/NPcap on 2020-08-11 23:17:33 UTC