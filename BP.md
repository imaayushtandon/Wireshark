# Bundle Protocol (BP)

The Bundle Protocol provides datagram transport over a high-delay or disrupted network using a variety of convergence layers ([TCPCL](/TCPCL), [LTPCL](/LTP), _etc._) to give per-hop bundle transport between BP nodes.

## History

BPv6 was standardized in RFC 5050.
BPv7 is currently finalizing IETF standardization.

## Protocol dependencies

  - [TCPCL](/TCPCL): The BPv6 had defined a corresponding TCPCL version 3 and BPv7 has defined a TCPCL version 4 to provide reliable transfer over low-delay (_e.g._ local or terresterial) data links.
  - [LTPCL](/LTP): The BPv6 has an experimental Licklider Transmission Protocol to provide reliable transfer over high-delay and lossy data links.
  - BPSec: The BP uses extension Block Integrity Block (BIB) and Block Confidentiality Block (BCB) to provide block-level security.

## Wireshark

The BP dissector supports extension block dissecting using sub-dissector tables and payload dissection as an administrative record or based on destination EID service identification.

## Preference Settings

The BPv7 dissector contains a heuristic dissector for block-type-specific data (BTSD) which currently uses CBOR as a fallback.

## Example capture file

The unit test tree contains a BPv6 and BPv7 test file using separate CLs. 

[[BPv6|/../blob/master/test/captures/dtn_tcpclv3_bpv6_transfer.pcapng]]

[[BPv7|/../blob/master/test/captures/dtn_tcpclv4_bpv7_transfer.pcapng]]


## Display Filter

Show only BPv6 traffic with filter `bundle` and BPv7 traffic with filter `bpv7`.

## External links

  - [RFC 5050](https://www.ietf.org/rfc/rfc5050.html) Bundle Protocol Specification
  - [draft-ietf-dtn-bpbis-31](https://datatracker.ietf.org/doc/html/draft-ietf-dtn-bpbis-31)