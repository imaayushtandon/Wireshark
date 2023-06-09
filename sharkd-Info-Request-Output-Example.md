The following is an example of the output from ```{"req":"info"}```.  The output is a single line with few whitespace characters.  The sample presented here has been formatted for easy viewing.

```
{
  "columns": [
    {
      "name": "802.1Q VLAN id",
      "format": "%q"
    },
    {
      "name": "Absolute date, as YYYY-MM-DD, and time",
      "format": "%Yt"
    },
    {
      "name": "Absolute date, as YYYY/DOY, and time",
      "format": "%YDOYt"
    },
    {
      "name": "Absolute time",
      "format": "%At"
    },
    {
      "name": "Cisco VSAN",
      "format": "%V"
    },
    {
      "name": "Cumulative Bytes",
      "format": "%B"
    },
    {
      "name": "Custom",
      "format": "%Cus"
    },
    {
      "name": "DCE/RPC call (cn_call_id / dg_seqnum)",
      "format": "%y"
    },
    {
      "name": "Delta time",
      "format": "%Tt"
    },
    {
      "name": "Delta time displayed",
      "format": "%Gt"
    },
    {
      "name": "Dest addr (resolved)",
      "format": "%rd"
    },
    {
      "name": "Dest addr (unresolved)",
      "format": "%ud"
    },
    {
      "name": "Dest port (resolved)",
      "format": "%rD"
    },
    {
      "name": "Dest port (unresolved)",
      "format": "%uD"
    },
    {
      "name": "Destination address",
      "format": "%d"
    },
    {
      "name": "Destination port",
      "format": "%D"
    },
    {
      "name": "Expert Info Severity",
      "format": "%a"
    },
    {
      "name": "FW-1 monitor if/direction",
      "format": "%I"
    },
    {
      "name": "Frequency/Channel",
      "format": "%F"
    },
    {
      "name": "Hardware dest addr",
      "format": "%hd"
    },
    {
      "name": "Hardware src addr",
      "format": "%hs"
    },
    {
      "name": "Hw dest addr (resolved)",
      "format": "%rhd"
    },
    {
      "name": "Hw dest addr (unresolved)",
      "format": "%uhd"
    },
    {
      "name": "Hw src addr (resolved)",
      "format": "%rhs"
    },
    {
      "name": "Hw src addr (unresolved)",
      "format": "%uhs"
    },
    {
      "name": "IEEE 802.11 RSSI",
      "format": "%e"
    },
    {
      "name": "IEEE 802.11 TX rate",
      "format": "%x"
    },
    {
      "name": "IP DSCP Value",
      "format": "%f"
    },
    {
      "name": "Information",
      "format": "%i"
    },
    {
      "name": "Net dest addr (resolved)",
      "format": "%rnd"
    },
    {
      "name": "Net dest addr (unresolved)",
      "format": "%und"
    },
    {
      "name": "Net src addr (resolved)",
      "format": "%rns"
    },
    {
      "name": "Net src addr (unresolved)",
      "format": "%uns"
    },
    {
      "name": "Network dest addr",
      "format": "%nd"
    },
    {
      "name": "Network src addr",
      "format": "%ns"
    },
    {
      "name": "Number",
      "format": "%m"
    },
    {
      "name": "Packet length (bytes)",
      "format": "%L"
    },
    {
      "name": "Protocol",
      "format": "%p"
    },
    {
      "name": "Relative time",
      "format": "%Rt"
    },
    {
      "name": "Source address",
      "format": "%s"
    },
    {
      "name": "Source port",
      "format": "%S"
    },
    {
      "name": "Src addr (resolved)",
      "format": "%rs"
    },
    {
      "name": "Src addr (unresolved)",
      "format": "%us"
    },
    {
      "name": "Src port (resolved)",
      "format": "%rS"
    },
    {
      "name": "Src port (unresolved)",
      "format": "%uS"
    },
    {
      "name": "TEI",
      "format": "%E"
    },
    {
      "name": "UTC date, as YYYY-MM-DD, and time",
      "format": "%Yut"
    },
    {
      "name": "UTC date, as YYYY/DOY, and time",
      "format": "%YDOYut"
    },
    {
      "name": "UTC time",
      "format": "%Aut"
    },
    {
      "name": "Time (format as specified)",
      "format": "%t"
    }
  ],
  "stats": [
    {
      "name": "29West/Queues/Advertisements by Queue",
      "tap": "stat:lbmr_queue_ads_queue"
    },
    {
      "name": "29West/Queues/Advertisements by Source",
      "tap": "stat:lbmr_queue_ads_source"
    },
    {
      "name": "29West/Queues/Queries by Queue",
      "tap": "stat:lbmr_queue_queries_queue"
    },
    {
      "name": "29West/Queues/Queries by Receiver",
      "tap": "stat:lbmr_queue_queries_receiver"
    },
    {
      "name": "29West/Topics/Advertisements by Source",
      "tap": "stat:lbmr_topic_ads_source"
    },
    {
      "name": "29West/Topics/Advertisements by Topic",
      "tap": "stat:lbmr_topic_ads_topic"
    },
    {
      "name": "29West/Topics/Advertisements by Transport",
      "tap": "stat:lbmr_topic_ads_transport"
    },
    {
      "name": "29West/Topics/Queries by Receiver",
      "tap": "stat:lbmr_topic_queries_receiver"
    },
    {
      "name": "29West/Topics/Queries by Topic",
      "tap": "stat:lbmr_topic_queries_topic"
    },
    {
      "name": "29West/Topics/Wildcard Queries by Pattern",
      "tap": "stat:lbmr_topic_queries_pattern"
    },
    {
      "name": "29West/Topics/Wildcard Queries by Receiver",
      "tap": "stat:lbmr_topic_queries_pattern_receiver"
    },
    {
      "name": "ANCP",
      "tap": "stat:ancp"
    },
    {
      "name": "BACnet/Packets sorted by IP",
      "tap": "stat:bacapp_ip"
    },
    {
      "name": "BACnet/Packets sorted by Instance ID",
      "tap": "stat:bacapp_instanceid"
    },
    {
      "name": "BACnet/Packets sorted by Object Type",
      "tap": "stat:bacapp_objectid"
    },
    {
      "name": "BACnet/Packets sorted by Service",
      "tap": "stat:bacapp_service"
    },
    {
      "name": "Collectd",
      "tap": "stat:collectd"
    },
    {
      "name": "DNS",
      "tap": "stat:dns"
    },
    {
      "name": "F5/Virtual Server Distribution",
      "tap": "stat:f5_virt_dist"
    },
    {
      "name": "F5/tmm Distribution",
      "tap": "stat:f5_tmm_dist"
    },
    {
      "name": "HART-IP",
      "tap": "stat:hart_ip"
    },
    {
      "name": "HPFEEDS",
      "tap": "stat:hpfeeds"
    },
    {
      "name": "HTTP/Load Distribution",
      "tap": "stat:http_srv"
    },
    {
      "name": "HTTP/Packet Counter",
      "tap": "stat:http"
    },
    {
      "name": "HTTP/Request Sequences",
      "tap": "stat:http_seq"
    },
    {
      "name": "HTTP/Requests",
      "tap": "stat:http_req"
    },
    {
      "name": "HTTP2",
      "tap": "stat:http2"
    },
    {
      "name": "Osmux/osmux",
      "tap": "stat:osmux"
    },
    {
      "name": "RTSP/Packet Counter",
      "tap": "stat:rtsp"
    },
    {
      "name": "SM_PP Operations",
      "tap": "stat:smpp_commands"
    },
    {
      "name": "Sametime/Messages",
      "tap": "stat:sametime"
    },
    {
      "name": "_ISUP Messages",
      "tap": "stat:isup_msg"
    },
    {
      "name": "_UCP Messages",
      "tap": "stat:ucp_messages"
    }
  ],
  "ftypes": [
    "FT_NONE",
    "FT_PROTOCOL",
    "FT_BOOLEAN",
    "FT_CHAR",
    "FT_UINT8",
    "FT_UINT16",
    "FT_UINT24",
    "FT_UINT32",
    "FT_UINT40",
    "FT_UINT48",
    "FT_UINT56",
    "FT_UINT64",
    "FT_INT8",
    "FT_INT16",
    "FT_INT24",
    "FT_INT32",
    "FT_INT40",
    "FT_INT48",
    "FT_INT56",
    "FT_INT64",
    "FT_IEEE_11073_SFLOAT",
    "FT_IEEE_11073_FLOAT",
    "FT_FLOAT",
    "FT_DOUBLE",
    "FT_ABSOLUTE_TIME",
    "FT_RELATIVE_TIME",
    "FT_STRING",
    "FT_STRINGZ",
    "FT_UINT_STRING",
    "FT_ETHER",
    "FT_BYTES",
    "FT_UINT_BYTES",
    "FT_IPv4",
    "FT_IPv6",
    "FT_IPXNET",
    "FT_FRAMENUM",
    "FT_PCRE",
    "FT_GUID",
    "FT_OID",
    "FT_EUI64",
    "FT_AX25",
    "FT_VINES",
    "FT_REL_OID",
    "FT_SYSTEM_ID",
    "FT_STRINGZPAD",
    "FT_FCWWN",
    "FT_STRINGZTRUNC"
  ],
  "version": "v3.5.0rc0-595-g0d820ddc8d2d",
  "nstat": [
    {
      "name": "A-I/F BSMAP Statistics",
      "tap": "nstat:ansi_a,bsmap"
    },
    {
      "name": "A-I/F DTAP Statistics",
      "tap": "nstat:ansi_a,dtap"
    },
    {
      "name": "Map Operation Statistics",
      "tap": "nstat:ansi_map"
    },
    {
      "name": "CAMEL Messages and Response Status",
      "tap": "nstat:camel,counter"
    },
    {
      "name": "DHCP (BOOTP) Statistics",
      "tap": "nstat:dhcp,stat"
    },
    {
      "name": "A-Interface BSSMAP",
      "tap": "nstat:gsm_a,bssmap"
    },
    {
      "name": "A-Interface DTAP Call Control",
      "tap": "nstat:gsm_a,dtap_cc"
    },
    {
      "name": "A-Interface DTAP GPRS Mobility Management",
      "tap": "nstat:gsm_a,dtap_gmm"
    },
    {
      "name": "A-Interface DTAP Mobility Management",
      "tap": "nstat:gsm_a,dtap_mm"
    },
    {
      "name": "A-Interface DTAP Radio Resource Management",
      "tap": "nstat:gsm_a,dtap_rr"
    },
    {
      "name": "A-Interface SACCH",
      "tap": "nstat:gsm_a,dtap_sacch"
    },
    {
      "name": "A-Interface DTAP GPRS Session Management",
      "tap": "nstat:gsm_a,dtap_sm"
    },
    {
      "name": "A-Interface DTAP Short Message Service",
      "tap": "nstat:gsm_a,dtap_sms"
    },
    {
      "name": "A-Interface DTAP Supplementary Services",
      "tap": "nstat:gsm_a,dtap_ss"
    },
    {
      "name": "A-Interface DTAP Special Conformance Testing Functions",
      "tap": "nstat:gsm_a,dtap_tp"
    },
    {
      "name": "MAP Operation",
      "tap": "nstat:gsm_map,operation"
    },
    {
      "name": "H.225",
      "tap": "nstat:h225,counter"
    },
    {
      "name": "MTP3 Statistics",
      "tap": "nstat:mtp3,msus"
    },
    {
      "name": "ONC-RPC Programs",
      "tap": "nstat:rpc,programs"
    },
    {
      "name": "SIP Statistics",
      "tap": "nstat:sip,stat"
    },
    {
      "name": "WAP-WSP Packet Counter",
      "tap": "nstat:wsp,stat"
    }
  ],
  "convs": [
    {
      "name": "Conversation List/Bluetooth",
      "tap": "conv:Bluetooth"
    },
    {
      "name": "Endpoint/Bluetooth",
      "tap": "endpt:Bluetooth"
    },
    {
      "name": "Conversation List/Ethernet",
      "tap": "conv:Ethernet"
    },
    {
      "name": "Endpoint/Ethernet",
      "tap": "endpt:Ethernet"
    },
    {
      "name": "Conversation List/FC",
      "tap": "conv:FC"
    },
    {
      "name": "Endpoint/FC",
      "tap": "endpt:FC"
    },
    {
      "name": "Conversation List/FDDI",
      "tap": "conv:FDDI"
    },
    {
      "name": "Endpoint/FDDI",
      "tap": "endpt:FDDI"
    },
    {
      "name": "Conversation List/IEEE 802.11",
      "tap": "conv:IEEE 802.11"
    },
    {
      "name": "Endpoint/IEEE 802.11",
      "tap": "endpt:IEEE 802.11"
    },
    {
      "name": "Conversation List/IEEE 802.15.4",
      "tap": "conv:IEEE 802.15.4"
    },
    {
      "name": "Endpoint/IEEE 802.15.4",
      "tap": "endpt:IEEE 802.15.4"
    },
    {
      "name": "Conversation List/IPX",
      "tap": "conv:IPX"
    },
    {
      "name": "Endpoint/IPX",
      "tap": "endpt:IPX"
    },
    {
      "name": "Conversation List/IPv4",
      "tap": "conv:IPv4"
    },
    {
      "name": "Endpoint/IPv4",
      "tap": "endpt:IPv4"
    },
    {
      "name": "Conversation List/IPv6",
      "tap": "conv:IPv6"
    },
    {
      "name": "Endpoint/IPv6",
      "tap": "endpt:IPv6"
    },
    {
      "name": "Conversation List/JXTA",
      "tap": "conv:JXTA"
    },
    {
      "name": "Endpoint/JXTA",
      "tap": "endpt:JXTA"
    },
    {
      "name": "Conversation List/MPTCP",
      "tap": "conv:MPTCP"
    },
    {
      "name": "Endpoint/MPTCP",
      "tap": "endpt:MPTCP"
    },
    {
      "name": "Conversation List/NCP",
      "tap": "conv:NCP"
    },
    {
      "name": "Endpoint/NCP",
      "tap": "endpt:NCP"
    },
    {
      "name": "Conversation List/RSVP",
      "tap": "conv:RSVP"
    },
    {
      "name": "Endpoint/RSVP",
      "tap": "endpt:RSVP"
    },
    {
      "name": "Conversation List/SCTP",
      "tap": "conv:SCTP"
    },
    {
      "name": "Endpoint/SCTP",
      "tap": "endpt:SCTP"
    },
    {
      "name": "Conversation List/SLL",
      "tap": "conv:SLL"
    },
    {
      "name": "Endpoint/SLL",
      "tap": "endpt:SLL"
    },
    {
      "name": "Conversation List/TCP",
      "tap": "conv:TCP"
    },
    {
      "name": "Endpoint/TCP",
      "tap": "endpt:TCP"
    },
    {
      "name": "Conversation List/Token-Ring",
      "tap": "conv:Token-Ring"
    },
    {
      "name": "Endpoint/Token-Ring",
      "tap": "endpt:Token-Ring"
    },
    {
      "name": "Conversation List/UDP",
      "tap": "conv:UDP"
    },
    {
      "name": "Endpoint/UDP",
      "tap": "endpt:UDP"
    },
    {
      "name": "Conversation List/USB",
      "tap": "conv:USB"
    },
    {
      "name": "Endpoint/USB",
      "tap": "endpt:USB"
    },
    {
      "name": "Conversation List/ZigBee",
      "tap": "conv:ZigBee"
    },
    {
      "name": "Endpoint/ZigBee",
      "tap": "endpt:ZigBee"
    }
  ],
  "seqa": [
    {
      "name": "All Flows",
      "tap": "seqa:any"
    },
    {
      "name": "ICMP Flows",
      "tap": "seqa:icmp"
    },
    {
      "name": "ICMPv6 Flows",
      "tap": "seqa:icmpv6"
    },
    {
      "name": "UIM Flows",
      "tap": "seqa:lbm_uim"
    },
    {
      "name": "TCP Flows",
      "tap": "seqa:tcp"
    }
  ],
  "taps": [
    {
      "name": "RTP streams",
      "tap": "rtp-streams"
    },
    {
      "name": "Expert Information",
      "tap": "expert"
    }
  ],
  "eo": [
    {
      "name": "Export Object/DICOM",
      "tap": "eo:dicom"
    },
    {
      "name": "Export Object/HTTP",
      "tap": "eo:http"
    },
    {
      "name": "Export Object/IMF",
      "tap": "eo:imf"
    },
    {
      "name": "Export Object/SMB",
      "tap": "eo:smb"
    },
    {
      "name": "Export Object/TFTP",
      "tap": "eo:tftp"
    }
  ],
  "srt": [
    {
      "name": "Service Response Time/AFP",
      "tap": "srt:afp"
    },
    {
      "name": "Service Response Time/CAMEL",
      "tap": "srt:camel"
    },
    {
      "name": "Service Response Time/DCERPC",
      "tap": "srt:dcerpc"
    },
    {
      "name": "Service Response Time/Diameter",
      "tap": "srt:diameter"
    },
    {
      "name": "Service Response Time/FC",
      "tap": "srt:fc"
    },
    {
      "name": "Service Response Time/GTP",
      "tap": "srt:gtp"
    },
    {
      "name": "Service Response Time/LDAP",
      "tap": "srt:ldap"
    },
    {
      "name": "Service Response Time/NCP",
      "tap": "srt:ncp"
    },
    {
      "name": "Service Response Time/RPC",
      "tap": "srt:rpc"
    },
    {
      "name": "Service Response Time/SCSI",
      "tap": "srt:scsi"
    },
    {
      "name": "Service Response Time/SMB",
      "tap": "srt:smb"
    },
    {
      "name": "Service Response Time/SMB2",
      "tap": "srt:smb2"
    },
    {
      "name": "Service Response Time/SNMP",
      "tap": "srt:snmp"
    }
  ],
  "rtd": [
    {
      "name": "Response Time Delay/H.225 RAS",
      "tap": "rtd:h225_ras"
    },
    {
      "name": "Response Time Delay/MEGACO",
      "tap": "rtd:megaco"
    },
    {
      "name": "Response Time Delay/MGCP",
      "tap": "rtd:mgcp"
    },
    {
      "name": "Response Time Delay/RADIUS",
      "tap": "rtd:radius"
    }
  ],
  "follow": [
    {
      "name": "Follow/HTTP",
      "tap": "follow:HTTP"
    },
    {
      "name": "Follow/HTTP2",
      "tap": "follow:HTTP2"
    },
    {
      "name": "Follow/QUIC",
      "tap": "follow:QUIC"
    },
    {
      "name": "Follow/TCP",
      "tap": "follow:TCP"
    },
    {
      "name": "Follow/TLS",
      "tap": "follow:TLS"
    },
    {
      "name": "Follow/UDP",
      "tap": "follow:UDP"
    }
  ]
}
```