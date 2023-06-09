The available sharkd request methods are:

- [analyse](#analyse)
- [bye](#bye)
- [check](#check)
- [complete](#complete)
- [download](#download)
- [dumpconf](#dumpconf)
- [follow](#follow)
- [frame](#frame)
- [frames](#frames)
- [info](#info)
- [intervals](#intervals)
- [iograph](#iograph)
- [load](#load)
- [setcomment](#setcomment)
- [setconf](#setconf)
- [status](#status)
- [tap](#tap)

See the [sharkd wiki page](sharkd-JSON-RPC) for an overview.

# analyse

Lists the protocols found in a packet file and its start and end times.

### Request

| Name | Value | Type | M/O |
|------|-------|------|-----|
| method | "analyse" | string | M |

M/O: M = Mandatory, O = Optional

NB: The request values is _analyse_ spelt in the UK English way - _analyze_ will not work.

### Response

| Name | Value | Type |
|------|-------|------|
| frames | Number of frames in the loaded file | integer |
| protocols | List of the protocol found in the loaded file | array of strings |
| first | Time of first entry in the capture | float representing epoch time and fractions of seconds |
| last | Time of last entry in the capture | float representing epoch time and fractions of seconds |

### Parameters

This request type has no other parameters.

### Examples
```
{"jsonrpc":"2.0","id":3,"method":"analyse"}
{"jsonrpc":"2.0","id":3,"result":{"frames":53882,"protocols":["frame","eth","ethertype","ip","tcp","pcoip","http","tds","data-text-lines","data","arp","udp","dns","dcerpc","cldap","spnego","spnego-krb5","ldap","gss-api","ipv6","dhcpv6","kerberos","nbdgm","smb","browser","nbss","smb2","urlencoded-form"],"first":1476281874.317326069,"last":1476283715.849661112}}
```
---

# bye

Ends the startd session.

If the request is sent in a Daemon Mode session connecting to sharkd, the connection terminates and the process for that session exits.  The daemon process continues to run and accept new connections.  Other existing sessions continues as normal.

If the request is sent in a Console Mode session, the sharkd process exits.

### Request

| Name | Value | Type | M/O |
|------|-------|------|-----|
| method | "bye" | string | M |

M/O: M = Mandatory, O = Optional

### Response

The response is a status result and then the application exits (in Console Mode) or the TCP connection terminates (in Daemon Mode).

### Examples
```
{"jsonrpc":"2.0","id":3,"method":"bye"}
{"jsonrpc":"2.0","id":3,"result":{"status":"OK"}}
```
---

# check

Check the validity of a field name or filter expression.

### Request

| Name | Value | Type | M/O |
|------|-------|------|-----|
| method | "check" | string | M |
| field | A fully qualified field reference | string | O |
| filter | A display filter expression | string | O |

M/O: M = Mandatory, O = Optional

### Response

| Name | Value | Type |
|------|-------|------|
| err | Error code - always 0 | integer |
| field | Return message | string |
| filter | Return message | string |

Return Message:

| Return Message | Description |
|----------------|-------------|
| "ok" | The field or filter is valid |
| Other values | As described in the return message  |

### Examples
```
{"jsonrpc":"2.0","id":3,"method":"check","params":{"field":"tcp.srcport"}}
{"jsonrpc":"2.0","id":3,"result":{"status":"OK"}}

{"jsonrpc":"2.0","id":5,"method":"check","params":{"filter":"tcp.dstport==80"}}
{"jsonrpc":"2.0","id":5,"result":{"status":"OK"}}

{"jsonrpc":"2.0","id":6,"method":"check","params":{"filter":"tcp.bad_field==80"}}
{"jsonrpc":"2.0","id":6,"error":{"code":-5001,"message":"Filter invalid - Neither \"tcp.bad_field\" nor \"80\" are field or protocol names."}}
```
---

# complete

Fetches the properties of a field(s) or preference(s).

### Request

| Name | Value | Type | M/O |
|------|-------|------|-----|
| method | "complete" | string | M |
| field | A fully qualified field reference | string | O |
| pref | A fully qualified preference reference | string | O |

M/O: M = Mandatory, O = Optional

The complete request assumes that the value specified for field or pref is the first part of its reference (i.e. prefix) and returns a list of every field that matches.  This is not a match based on the dotted hierarchy but a straightforward string match, e.g:

"field":"http.request" will return details for http.request_number as well as http.request.line, http.request.method, etc.

### Response

| Name | Value | Type |
|------|-------|------|
| err | Error code - always 0 | integer |
| field | "f":"_field_reference_"<br/>"t":_field_type_<br>"n":"_field_name_" | array of objects |
| pref| "f":"_preference_name_"<br/>"d":_preference_description_ | array of objects |

The field_type is a numeric value determined by an enumerated list - see [ftypes.h](https://gitlab.com/wireshark/wireshark/-/blob/master/epan/ftypes/ftypes.h) 

If the input field or pref values is incorrect, an empty array is returned.

### Examples
```
{"jsonrpc":"2.0","id":9,"method":"complete","params":{"field":"http.request.method"}}
{"jsonrpc":"2.0","id":9,"result":{"field":[{"f":"http.request.method","t":26,"n":"Request Method"}]}}

{"jsonrpc":"2.0","id":9,"method":"complete","params":{"field":"http.request"}}
{"jsonrpc":"2.0","id":9,"result":{"field":[{"f":"http.request","t":2,"n":"Request"},{"f":"http.request_number","t":7,"n":"Request number"},{"f":"http.request.line","t":26,"n":"Request line"},{"f":"http.request.method","t":26,"n":"Request Method"},{"f":"http.request.uri","t":26,"n":"Request URI"},{"f":"http.request.uri.path","t":26,"n":"Request URI Path"},{"f":"http.request.uri.query","t":26,"n":"Request URI Query"},{"f":"http.request.uri.query.parameter","t":26,"n":"Request URI Query Parameter"},{"f":"http.request.version","t":26,"n":"Request Version"},{"f":"http.request.full_uri","t":26,"n":"Full request URI"},{"f":"http.request_in","t":35,"n":"Request in frame"}]}}

{"jsonrpc":"2.0","id":9,"method":"complete","params":{"field":"http.bad_ref"}}
{"jsonrpc":"2.0","id":9,"result":{"field":[]}}

{"jsonrpc":"2.0","id":9,"method":"complete","params":{"pref":"tcp"}}
{"jsonrpc":"2.0","id":9,"result":{"pref":[{"f":"tcp","d":"TCP"},{"f":"tcpencap","d":"TCPENCAP"},{"f":"tcpros","d":"TCPROS"}]}}
```
---

# download

Get decoded objects (exported objects, SSL secrets or rtp data); some downloaded data is base64 encoded.

### Request

| Name | Value | Type | M/O |
|------|-------|------|-----|
| method | "download" | string | M |
| token | Token to download:<br/>- eo:\<object_ref\><br/>- ssl-secrets<br/>- rtp:\<stream_specification\> | string | M |

M/O: M = Mandatory, O = Optional

Details of the object_ref and stream_specification are under eo and rtp respectively below.

#### eo

For the eo (Export Object) token we can download data objects of the following types:

- DICOM
- HTTP
- IMF
- SMB
- TFTP

Before requesting a download for an exported object we must export it.  This is done by running an appropriate tap command, for example:
```
{"jsonrpc":"2.0","id":3,"method":"tap", "params":{"tap0":"eo:http"}}
```
then we can make the request like this:
```
{"jsonrpc":"2.0","id":3,"method":"download", "params":{"token":"eo:http_0"}}
```

The __object_ref__ is created by suffixing the type with an underscore character (_) followed by an index into the table of detected objects.  In Wireshark, if we display the HTTP Export Objects for a sample file we may see something like this:

![export_object_screenshot](uploads/b882b04fb39f1c9752c9103b70ef02d2/export_object_screenshot.png)

To get the first object, we would use the request:
```
{"jsonrpc":"2.0","id":3,"method":"download", "params":{"token":"eo:http_0"}}
```
To get the second object, we would use the request:
```
{"jsonrpc":"2.0","id":3,"method":"download", "params":{"token":"eo:http_1"}}
```
And so on.

#### ssl-secrets

sharkd can export and download the session keys which are specific to the traffic in a capture file.  To extract the session keys we must first decrypt the traffic using the server private (RSA) key.  This is done by passing in the private key via the TLS preferences; either using Wireshark to edit the prefs file, or by using the sharkd setconf request prior to loading the trace file.

Once the file is decrypted, we can download the Session Keys (ssl-secrets).

See the [TLS page](TLS) of this wiki for further information.

#### rtp

With this option we can download the audio content from an rtp stream as an x-wav MIME data file.  The request must contain the stream_specification which uniquely identifies the stream as follows:

_source-ip_\__source-port_\__destination-ip_\__destination-port_\__synchronization-source-identifier_

A full request would then look like this:
```
{"jsonrpc":"2.0","id":3,"method":"download", "params":{"token":"rtp:200.57.7.204_8000_200.57.7.196_40376_0xd2bd4e3e"}}
```

### Response

| Name | Value | Type |
|------|-------|------|
| file | Suggested file name | string  |
| mime | The objects MIME content-type | string  |
| data | Base64-encoded object | string |

NB: If a request is made for a token that doesn't exist, sharkd responds with ```\r\n\r\n``` only i.e. there is no JSON response.

#### eo

The exported object can be an HTML page, a jpg image, a CSS stylesheet, etc.  The file name is equivalent to the final element of a URL path such as _index.html_ or _favicon.ico_

If we try to download an exported object that doesn't exist, there will be no response from sharkd.

#### ssl-secret

Output format is:
```
RSA Session-ID:xxxx Master-Key:yyyy\n
```
Where:
- xxxx is the session ID in hex (max 64 chars)
- yyyy is the Master Key in hex (always 96 chars)

So in total max 3+1+11+64+1+11+96+2 = 189 chars

Alternatively, the output is:
```
CLIENT_RANDOM zzzz yyyy\n
```
Where:
- zzzz is the client random (always 64 chars)
- yyyy is same as above

So length will always be 13+1+64+1+96+2 = 177 chars

#### rtp

The file value for an rtp download is the string "rtp:" suffixed with the stream specification.  See the rtp example below.

### Examples
```
{"jsonrpc":"2.0","id":3,"method":"download", "params":{"token":"eo:http_0"}}
{"jsonrpc":"2.0","id":3,"result":{"file":"About","mime":"text/html","data":"PCFET0NUWV ... ib2R5Pg0KPC9odG1sPg0K"}}

{"jsonrpc":"2.0","id":4,"method":"download", "params":{"token":"rtp:200.57.7.204_8000_200.57.7.196_40376_0xd2bd4e3e"}}
{"jsonrpc":"2.0","id":4,"result":{"file":"rtp:200.57.7.204_8000_200.57.7.196_40376_0xd2bd4e3e","mime":"audio/x-wav","data":"UklGRv////9XQVZF ... AQQAsj/eABwA5AC6P4Y/8gB8AJQAzAC"}}
```
---

# dumpconf

Lists one or all configuration parameters.

### Request

| Name | Value | Type | M/O |
|------|-------|------|-----|
| method | "dumpconf" | string | M |
| pref | A fully qualified preference reference | string | O |

M/O: M = Mandatory, O = Optional

If the pref value is not specified, all preferences are listed.

### Response

| Name | Objects in the Array | Type |
|------|-------|------|
| prefs |                      | array of objects |
|       | _pref_name_          | |
|       | "b":_binary_value_   | 0 - not set, 1 - set |
|       | "d":"_description_" | string |
|       | "e":[_Drop_down_list_]<br/>- "v":_value_<br/>- "s":_selected_<br/>- "d":"_description_" | array of objects |
|       | "r":"[_range_of_values_]" | string with a<br/>comma separated list of numbers<br/>or a range specified as<br/>_start_-_end_ |
|       | "s":"_string_value_" | string |
|       | "t":[_table_of_values_] | array of objects |
|       | "u":_unsigned_integer_value_ | integer |

### Examples
```
{"jsonrpc":"2.0","id":4,"method":"dumpconf", "params":{"pref":"tcp.desegment_tcp_streams"}}
{"jsonrpc":"2.0","id":4,"result":{"prefs":{"tcp.desegment_tcp_streams":{"b":1}}}}

{"jsonrpc":"2.0","id":5,"method":"dumpconf"}
{"jsonrpc":"2.0","id":5,"result":{"prefs": ...
"ber.decode_primitive":{"b":0}, ...
"bgp.asn_len":{"e":[{"v":0,"s":1,"d":"Auto-detect"},{"v":2,"d":"2 octet"},{"v":4,"d":"4 octet"}]} ...
"bjnp.udp.port":{"r": "8611-8614"}, ...
"couchbase.tls.port":{"u": 11207}, ...
"couchbase.tcp.port":{"r": "11210"}, ...
```
---

# follow

Get client and server information for a particular protocol or stream plus the data payload being carried by the protocol specified.  The protocol payload is JSON-Base64 encoded to accommodate binary content.

### Request

| Name | Value | Type | M/O |
|------|-------|------|-----|
| method | "follow" | string | M |
| follow | Protocol payload to output | string | M |
| filter | Filter expression | string | M |

M/O: M = Mandatory, O = Optional

### Response

| Name  | Value      | Type |
|-------|------------|------|
| err   | Error code | integer |
| shost | Service IP address | string (dotted IP address) |
| sport | Service port number | string |
| sbytes | Total number of bytes from service to client | integer |
| chost | Client IP address | string (dotted IP address) |
| cport | Client port number | string |
| cbytes | Total number of bytes from client to service | integer |
| payloads | The payload carried by the protocol<br/>specified in the packets in the stream | array of objects | 
| - "n": | Number of bytes in the payload | integer |
| - "d": | Protocol payload | bytes encoded as JSON-Base64 |
| - "s": | Direction of the flow<br/>missing - client to service<br/>1 - service to client | integer |

Bear in mind that this request will deliver all the data in a stream and so the response may be very large.

### Examples
```
{"jsonrpc":"2.0","id":4,"method":"follow", "params":{"follow":"HTTP","filter":"tcp.stream==0"}}
{"jsonrpc":"2.0","id":4,"result":{"shost":"192.168.3.78","sport":"80","sbytes":110,"chost":"192.168.3.85","cport":"46815","cbytes":5339,"payloads":[{"n":4,"d":"R0VUIC9NeUFwcC9Ib21lL0Fib3V ... 5NQ0KDQo=","s":1},{"n":9,"d":"PCFET0 ... KPC9odG1sPg0K","s":1}]}}

{"jsonrpc":"2.0","id":5,"method":"follow", "params":{"follow":"TCP","filter":"tcp.stream==1"}}
{"jsonrpc":"2.0","id":5,"result":{"shost":"192.168.3.79","sport":"1433","sbytes":163222,"chost":"192.168.3.78","cport":"50442","cbytes":66745,"payloads":[{"n":5,"d":"AQkBBAAAA ... hAHQAZQA="},{"n":6,"d":"BAEBCQBH ... AYAAAA=","s":1},{"n":22741,"d":"BgAAABoAQwByAGUAYQB0AGkAdgBlACAAQQByAHQAcwAAAAAAALb7XKyOAAAAAAAABAIAAAAIAAAAAAAAAA+q/xEAwQAKAAAAAAAAAHkAAAAA/gAA4AAAAAAAAAAAAA==","s":1}]}}

{"jsonrpc":"2.0","id":6,"method":"follow", "params":{"follow":"TCP","filter":"tcp.stream==10000"}}
{"jsonrpc":"2.0","id":6,"result":{"shost":"NONE","sport":"0","sbytes":0,"chost":"NONE","cport":"0","cbytes":0}}
```
---

# frame

Get full information about a frame including the protocol tree.

### Request

| Name | Value | Type | M/O |
|------|-------|------|-----|
| method | "frame" | string | M |
| frame | Frame number | integer | M |
| proto | If present, output the protocol tree | Any valid JSON value | O |
| ref_frame | If present, output the time reference frame number<br/>This doesn't seem to work | Any valid JSON value | O |
| prev_frame | If present, output the previously displayed frame number<br/>This doesn't seem to work | Any valid JSON value | O |
| columns | If present, output frame columns | Any valid JSON value | O |
| color | If present, output color-filter bg/fg | Any valid JSON value | O |
| bytes | If present, output frame bytes | Any valid JSON value | O |
| hidden | If present, output hidden tree fields | Any valid JSON value | O |

NB: A value of _true_ is acceptable for any field that has a type of _Any valid JSON value_.  However, a value of false is also treated as true i.e. ```{"jsonrpc":"2.0","id":6,"method":"frame","params":{"frame":4, "bytes":false}}``` will output the frame bytes.

M/O: M = Mandatory, O = Optional

### Response

| Name  | Value      | Type |
|-------|------------|------|
| err   | Error code | integer |
| tree  | Protocol tree information - see below | array of objects |
| col   | array of column data | array |
| bytes | Frame bytes encoded with Base64 | string of base64 |
| ds    | Other data sources | array of objects |
| comment | Frame comment | string |
| fol   | Follow filter:<br/>[0] - protocol<br/>[1] - filter string | array of objects |
| i     | true if frame is ignored | boolean |
| m     | true if frame is marked | boolean |
| bg    | Color filter - background color in hex | string |
| fg    | Color filter - foreground color in hex | string |


**Values in the tree array**

| Name | Value | Type |
|-------|------------|------|
| l    | Field label | string | 
| t    | Type of tree or subtree node - "proto", "framenum" or "url" | string
| f    | Filter variable (variable that can be used in an expression) | string |
| s    | Severity e.g. "Chat" | string |
| e    | Subtree ett index | integer |
| n    | Array of subtree nodes | array of objects |
| h    | Two item array: (item start, item length) | array of integers |
| i    | Two item array: (appendix start, appendix length) | array of integers |
| p    | [RESERVED] two item array: (protocol start, protocol length) | array of integers |
| ds   | Data src index
| url  | URL | string |
| fnum | Used to reference a frame elsewhere in the file | integer |
| g    | true if field is generated by Wireshark | boolean |
| v    | true if field is hidden | boolean |

### Examples
```
{"jsonrpc":"2.0","id":6,"method":"frame", "params":{"frame":4}}
{"jsonrpc":"2.0","id":6,"result":{"fol":[["HTTP","tcp.stream eq 0"],["TCP","tcp.stream eq 0"]]}}

{"jsonrpc":"2.0","id":7,"method":"frame", "params":{"frame":4, "proto":true}}
{"jsonrpc":"2.0","id":6,"result":{"tree":[{"l":"Frame 4: 176 bytes on wire (1408 bits), 176 bytes captured (1408 bits) on interface \\Device\\NPF_{304D305E-652F-47CD-B730-94986169FE76}, id 0","h":[0,176],"t":"proto","f":"frame","e":10538,"n":[{"l":"Interface id: 0 (\\Device\\NPF_{304D305E-652F-47CD-B730-94986169FE76})","f":"frame.inter ... rue},{"l":"Req Spread: 0.000000000 seconds","f":"transum.reqspread == 0.000000000","g":true},{"l":"Rsp Spread: 0.000164000 seconds","f":"transum.rspspread == 0.000164000","g":true},{"l":"Trace clip filter: tcp.stream==0 && frame.number>=4 && frame.number<=9 && tcp.len>0","f":"transum.clip_filter == \"tcp.stream==0 && frame.number>=4 && frame.number<=9 && tcp.len>0\"","g":true},{"l":"Calculation: Generic TCP","f":"transum.calculation == \"Generic TCP\"","g":true}]}],"fol":[["HTTP","tcp.stream eq 0"],["TCP","tcp.stream eq 0"]]}}

{"jsonrpc":"2.0","id":8,"method":"frame", "params":{"frame":4, "columns":true}}
{"jsonrpc":"2.0","id":6,"result":{"col":["4","0.000319","192.168.3.85","192.168.3.78","HTTP","176","GET /MyApp/Home/About HTTP/1.1 "],"fol":[["HTTP","tcp.stream eq 0"],["TCP","tcp.stream eq 0"]]}}

{"jsonrpc":"2.0","id":9,"method":"frame", "params":{"frame":4, "color":true}}
{"jsonrpc":"2.0","id":6,"result":{"bg":"e4ffc7","fg":"12272e","fol":[["HTTP","tcp.stream eq 0"],["TCP","tcp.stream eq 0"]]}}

{"jsonrpc":"2.0","id":10,"method":"frame", "params":{"frame":4, "bytes":"true"}}
{"jsonrpc":"2.0","id":6,"result":{"bytes":"AAwp+/kTAAwp2dO1CABFAACimmFAAEAGGAHAqANVwKgDTrbfAFAefnCL/KwyboAYAOU0GwAAAQEICgSv6tUOKKxsR0VUIC9NeUFwcC9Ib21lL0Fib3V0IEhUVFAvMS4xDQpDb25uZWN0aW9uOiBjbG9zZQ0KVXNlci1BZ2VudDogSmFrYXJ0YSBDb21tb25zLUh0dHBDbGllbnQvMy4xDQpIb3N0OiB3ZWIwMQ0KDQo=","fol":[["HTTP","tcp.stream eq 0"],["TCP","tcp.stream eq 0"]]}}
```
---

# frames

Get Packet List information for a range of packets.

### Request

| Name | Value | Type | M/O |
|------|-------|------|-----|
| method | "frames" | string | M |
| column0...columnXX | Requested columns either number in range [0..NUM_COL_FMTS], or custom (syntax \<dfilter\>:\<occurence\>).<br/><br/>If column0 is not specified, the current profile column set will be used. | integer or string | O |
| filter | Output those frames that pass this filter expression | string | O |
| skip | Skip N frames | integer | O |
| limit | Limit the output to N frames | integer | O |
| refs | Output based on this list (comma separated) of sorted time reference frame numbers. | string | O |

M/O: M = Mandatory, O = Optional

For details regarding the content of each column, see the columns listed on [sharkd Info Request Output Example](sharkd-Info-Request-Output-Example) wiki page.

### Response

The response is an array of objects.  The elements of the object are as follows:

| Name  | Value      | Type |
|-------|------------|------|
| c     | Output of columns as strings | array of strings |
| num   | Frame number | integer |
| bg    | Color filter - background color in hex | string |
| fg    | Color filter - foreground color in hex | string |

### Examples
```
{"jsonrpc":"2.0","id":4,"method":"frames", "params":{"filter":"frame.number<=2"}}
{"jsonrpc":"2.0","id":4,"result":{[{"c":["1","0.000000","192.168.3.85","192.168.3.78","TCP","74","46815 ÔåÆ 80 [SYN] Seq=0 Win=29200 Len=0 MSS=1460 SACK_PERM=1 TSval=78637781 TSecr=0 WS=128"],"num":1,"bg":"e4ffc7","fg":"12272e"},{"c":["2","0.000075","192.168.3.78","192.168.3.85","TCP","74","80 ÔåÆ 46815 [SYN, ACK] Seq=0 Ack=1 Win=8192 Len=0 MSS=1460 WS=256 SACK_PERM=1 TSval=237546604 TSecr=78637781"],"num":2,"bg":"e4ffc7","fg":"12272e"}]}

{"jsonrpc":"2.0","id":5,"method":"frames", "params":{"limit":2}
{"jsonrpc":"2.0","id":5,"result":{[{"c":["1","0.000000","192.168.3.85","192.168.3.78","TCP","74","46815 ÔåÆ 80 [SYN] Seq=0 Win=29200 Len=0 MSS=1460 SACK_PERM=1 TSval=78637781 TSecr=0 WS=128"],"num":1,"bg":"e4ffc7","fg":"12272e"},{"c":["2","0.000075","192.168.3.78","192.168.3.85","TCP","74","80 ÔåÆ 46815 [SYN, ACK] Seq=0 Ack=1 Win=8192 Len=0 MSS=1460 WS=256 SACK_PERM=1 TSval=237546604 TSecr=78637781"],"num":2,"bg":"e4ffc7","fg":"12272e"}]}

{"jsonrpc":"2.0","id":6,"method":"frames", "params":{"skip":2,"limit":2}
{"jsonrpc":"2.0","id":6,"result":{[{"c":["3","0.000195","192.168.3.85","192.168.3.78","TCP","66","46815 ÔåÆ 80 [ACK] Seq=1 Ack=1 Win=29312 Len=0 TSval=78637781 TSecr=237546604"],"num":3,"bg":"e4ffc7","fg":"12272e"},{"c":["4","0.000319","192.168.3.85","192.168.3.78","HTTP","176","GET /MyApp/Home/About HTTP/1.1 "],"num":4,"bg":"e4ffc7","fg":"12272e"}]}

{"jsonrpc":"2.0","id":7,"method":"frames", "params":{"skip":2,"limit":1, "column0":20, "column1":"transum.art:1"}}
{"jsonrpc":"2.0","id":7,"result":{[{"c":["VMware_d9:d3:b5","0.000075000"],"num":1,"bg":"e4ffc7","fg":"12272e"}]}
```
---

### Notes

There are a few considerations when using the frames request:

- The columns must be predefined in the Default profile
- Values for hidden columns are included in the response
- After making a change to the columns with Wireshark, we must:
  + Close Wireshark to save the change
  + Restart sharkd, including the whole daemon (if using Daemon Mode) not just the running session
- The columns are just listed as quoted comma separated variables
- There are no labels or field names associated with the columns

# info

Get a list of format and statistics information types available to sharkd clients.

### Request

| Name | Value | Type | M/O |
|------|-------|------|-----|
| method | "info" | string | M |

M/O: M = Mandatory, O = Optional

### Response

| Name | Value | Type |
|------|-------|------|
| columns |  | array of objects |
| stats   |  | array of objects |
| ftypes  |  | array of objects |
| version |  | string |
| nstat   |  | array of objects |
| convs   |  | array of objects |
| sequ    |  | array of objects |
| taps    |  | array of objects |
| eo      |  | array of objects |
| srt     |  | array of objects |
| rtd     |  | array of objects |
| follow  |  | array of objects |

For a complete example of the output from the info request see the [sharkd Info Request Output Example](sharkd-Info-Request-Output-Example) wiki page.

To be completed

### Examples
```
{"jsonrpc":"2.0","id":6,"method":"info"}
{"jsonrpc":"2.0","id":7,"result":{{"columns":[{"name":"802.1Q VLAN id","format":"%q"},
...
"stats":[{"name":"29West/Queues/Advertisements by Queue","tap":"stat:lbmr_queue_ads_queue"},
...
"ftypes":["FT_NONE","FT_PROTOCOL","FT_BOOLEAN","FT_CHAR","FT_UINT8","FT_UINT16","FT_UINT24","FT_UINT32",
...
"version":"v3.5.0rc0-595-g0d820ddc8d2d",
"nstat":[{"name":"A-I/F BSMAP Statistics","tap":"nstat:ansi_a,bsmap"},
...
"convs":[{"name":"Conversation List/Bluetooth","tap":"conv:Bluetooth"},
...
"seqa":[{"name":"All Flows","tap":"seqa:any"},
...
"taps":[{"name":"RTP streams","tap":"rtp-streams"},{"name":"Expert Information","tap":"expert"}],
"eo":[{"name":"Export Object/DICOM","tap":"eo:dicom"},
...
"srt":[{"name":"Service Response Time/AFP","tap":"srt:afp"},
...
"rtd":[{"name":"Response Time Delay/H.225 RAS","tap":"rtd:h225_ras"},
...
"follow":[{"name":"Follow/HTTP","tap":"follow:HTTP"}, ... }
```

---

# intervals

This request considers aggregates the packet data to produce a count of the number of frames and a sum of the number of bytes in each interval.  The aggregation is performed on fixed time intervals (default is one second).

### Request

| Name | Value | Type | M/O |
|------|-------|------|-----|
| method | "intervals" | string | M |
| interval | Interval time in milliseconds | integer | O |
| filter | Display filter term applied prior to producing the sample set | string | O |

M/O: M = Mandatory, O = Optional

### Response

| Name | Value | Type |
|------|-------|------|
| intervals | Data array of arrays of integer values in the format [x,y,z] where:<br/>x - interval number<br/>y - number of frames in this interval<br/>z - number of bytes in this interval | an array of arrays of comma separated integers |
| last | The last interval number in the sample | integer |
| frames | The total number of frames in the sample | integer |
| bytes | The total number of bytes in the sample | integer |

NB: If there are no packets within an interval, no values are generated for that interval

### Examples
```
{"jsonrpc":"2.0","id":6,"method":"intervals", "params":{"filter":"frame.number<=60"}}
{"jsonrpc":"2.0","id":7,"result":{"intervals":[[0,13,6812],[1,38,31459],[2,9,3775]],"last":2,"frames":60,"bytes":42046}}

{"jsonrpc":"2.0","id":6,"method":"intervals", "params":{"interval":100,"filter":"frame.number<=60"}}
{"jsonrpc":"2.0","id":7,"result":{"intervals":[[0,12,6758],[1,1,54],[10,15,14783],[12,23,16676],[20,9,3775]],"last":20,"frames":60,"bytes":42046}}
```
The output in the final example has intervals missing because there were no packets within these intervals.

---

# iograph

Creates time sequenced list of values for graphing; default is second-by-second.

### Request

| Name | Value | Type | M/O |
|------|-------|------|-----|
| method | "iographs" | string | M |
| interval | Interval time in milliseconds | integer | O |
| filter | Display filter term applied prior to producing the sample set | string | O |
| graph0 | First graph request - see below for details | string | M |
| graph1...graph9 | Other graph requests - see below for details | string | O |
| filter0 | First graph filter | string | O |
| filter1...filter9 | Other graph filters | string | O |

M/O: M = Mandatory, O = Optional

Graph requests:

| Name | Value | Type |
|------|-------|------|
| graph0...graph9 | "packets" | string |
| graph0...graph9 | "bytes" | string |
| graph0...graph9 | "bits" | string |
| graph0...graph9 | "sum:\<field\>" | string |
| graph0...graph9 | "frames:\<field\>" | string |
| graph0...graph9 | "max:\<field\>" | string |
| graph0...graph9 | "min:\<field\>" | string |
| graph0...graph9 | "avg:\<field\>" | string |
| graph0...graph9 | "load:\<field\>" | string |

**NB:** Whichever field we want to graph must appear in the corresponding filter expression.  If it doesn't, no values are generated.

An attempt to graph a field that is not a numeric produces an empty output array.

### Response

| Name | Value | Type |
|------|-------|------|
| iograph | The top level array of output objects, one for each graph | an array of objects |
| items | An array of values for one of the graphs specified | integer |

NB: If there are no packets within an interval, no values are generated for that interval

### Examples
```
{"jsonrpc":"2.0","id":6,"method":"iograph", "params":{"graph0":"packets","filter0":"frame.number<=100"}}
{"jsonrpc":"2.0","id":7,"result":{"iograph":[{"items":[13.000000,38.000000,23.000000,25.000000,1.000000]}]}

{"jsonrpc":"2.0","id":6,"method":"iograph", "params":{"graph0":"sum:tcp.len","filter0":"http && frame.number<=100 && tcp.len"}}
{"jsonrpc":"2.0","id":7,"result":{"iograph":[{"items":[2553.000000,4640.000000,955.000000,5416.000000]}]}}

{"jsonrpc":"2.0","id":6,"method":"iograph", "params":{"graph0":"sum:frame.len","filter0":"http && frame.number<=100 && frame.len","graph1":"sum:tcp.len","filter1":"http && frame.number<=100 && tcp.len"}}
{"jsonrpc":"2.0","id":7,"result":{"iograph":[{"items":[2685.000000,4904.000000,1153.000000,5614.000000]},{"items":[2553.000000,4640.000000,955.000000,5416.000000]}]}}

{"jsonrpc":"2.0","id":6,"method":"iograph", "params":{"graph0":"http.request","filter0":"http && frame.number<=100 && http.request"}}
{"jsonrpc":"2.0","id":7,"result":{"iograph":[]}}
```

---

# load

Load a packet trace file for analysis.

### Request

| Name | Value | Type | M/O |
|------|-------|------|-----|
| method | "load" | string | M |
| file | Path and name of the file to be loaded | string | M |

M/O: M = Mandatory, O = Optional

### Response

| Name | Value | Type |
|------|-------|------|
| err | Error code | integer |

Error Codes:

| Error Code | Description |
|------|-------------------|
| 0 | The operation was successful |
| 2 | The file doesn't exist |

### Examples
```
{"jsonrpc":"2.0", "id":1, "method":"load", "params":{"file":"c:/traces/Contoso_01/web01/web01_00001_20161012151754.pcapng"}}
{"jsonrpc":"2.0","id":1,"result":{"status":"OK"}}

{"jsonrpc":"2.0", "id":2, "method":"load", "params":{"file":"c:/traces/Contoso_01/web01/wrong_file_name.pcapng"}}
{"jsonrpc":"2.0","id":2,"error":{"code":-2001,"message":"Unable to open the file"}}
```
---

# setcomment

Sets a comment in a frame for the duration of a sharkd session i.e. the PCAPNG file is not modified and so the comment is not persistent.

### Request

| Name | Value | Type | M/O |
|------|-------|------|-----|
| method | "setcomment" | string | M |
| frame | The frame in which the comment is set | integer | M |
| comment | The comment text | string | O |

M/O: M = Mandatory, O = Optional

### Response

| Name | Value | Type |
|------|-------|------|
| err | Error code | integer |

Error Codes:

| Error Code | Description |
|------|-------------------|
| 0 | The operation was successful |

### Examples
```
{"jsonrpc":"2.0","id":6,"method":"setcomment", "params":{"frame":1,"comment":"Hello world"}
{"jsonrpc":"2.0","id":1,"result":{"status":"OK"}}

{"jsonrpc":"2.0","id":6,"method":"setcomment", "params":{"frame":1, "proto":"true"}}
{"jsonrpc":"2.0","id":1,"result":{"comment":"Hello world","tree":[{"l":"Packet comments","t":"proto","f":"pkt_comment","s":"Comment","e":10541,"n":[{"l":"Hello world","f":"frame.comment == \"Hello world\"","s":"Comment","e":55379,"n":[{"l":"Expert Info (Comment/Comment): Hello world","t":"pro ...
```
---

# setconf

Set a preference for the duration of a sharkd session i.e. the preference file is not modified and so the setting is not persistent.

### Request

| Name  | Value | Type | M/O |
|-------|-------|------|-----|
| req   | "setconf" | string | M |
| name  | The name of the preference | string | M |
| value | The new value for the preference | as appropriate | M |

M/O: M = Mandatory, O = Optional

### Response

| Name | Value | Type |
|------|-------|------|
| err | Error code | integer |

Error Codes:

| Error Code | Description |
|------|-------------------|
| 0 | The operation was successful |

### Examples
```
{"jsonrpc":"2.0","id":6,"method":"setconf", "params":{"name":"tcp.desegment_tcp_streams","value":false}}
{"jsonrpc":"2.0","id":1,"result":{"status":"OK"}}

{"jsonrpc":"2.0","id":6,"method":"dumpconf", "params":{"pref":"tcp.desegment_tcp_streams"}}
{"jsonrpc":"2.0","id":1,"result":{"tcp.desegment_tcp_streams":{"b":0}}}

{"jsonrpc":"2.0","id":6,"method":"setconf", "params":{"name":"tcp.desegment_tcp_streams","value":true}}
{"jsonrpc":"2.0","id":1,"result":{"status":"OK"}}

{"jsonrpc":"2.0","id":6,"method":"dumpconf", "params":{"pref":"tcp.desegment_tcp_streams"}}
{"jsonrpc":"2.0","id":1,"result":{"tcp.desegment_tcp_streams":{"b":1}}}
```
---

# status

Get basic information about the loaded file (name, size, number of frames, etc.).

### Request

| Name | Value | Type | M/O |
|------|-------|------|-----|
| method | "status" | string | M |

M/O: M = Mandatory, O = Optional

### Response

| Name | Value | Type |
|------|-------|------|
| frames | Count of currently loaded frames | integer |
| duration | Time difference between time of first frame, and last loaded frame | number |
| filename | Capture file name - only present if file is loaded | string |
| filesize | Capture file size  - only present if file is loaded | integer |

### Examples
```
{"jsonrpc":"2.0","id":6,"method":"status"}
{"jsonrpc":"2.0","id":1,"result":{{"frames":53882,"duration":1841.532335000,"filename":"web01_00001_20161012151754.pcapng","filesize":36433896}}
```
---

# tap

Set up to 16 statistics taps and get statistics from them; tap types are stats, nstat, conv, host, rtp-streams, rtp-analyse, eo, expert, rtd, srt and flow.

### Request

| Name | Value | Type | M/O |
|------|-------|------|-----|
| method | "tap" | string | M |
| tap0 | First tap type request | string | M | 
| tap1 ... tap15 | Other tap type request | string | O | 

M/O: M = Mandatory, O = Optional

**Tap Types**

The format of this attribute is ```"tap0":"<type>:<subtype>"```

There are many types and subtypes - too many to list here and the list will change as new protocols are added to Wireshark.  Use the __info__ request to get a full list of the values available - [click here for example info output](sharkd-Info-Request-Output-Example).   

### Response

| Name | Value | Type |
|------|-------|------|
| err | Error code | integer |
| taps | One array for each tap specified | array of objects |
| - tap | Tap type - see above | string |
| - type | Tap type - see above | string |
| - details | A array of objects, one for each packet | array of objects |

Objects in the details fields:

| Name | Value | Type |
|------|-------|------|
| **conv** | |
| | |
| **eo** | |
| | |
| **flow** | |
| | |
| **host** | |
| | |
| **expert** | |      |
| f    | Frame number | integer |
| g    | Expert group | string |
|      | "Sequence" |
| m    | Expert message | string |
| p    | Protocol that this message applies to | string |
| s    | Severity of this message | string |
|      | "Chat" |
| **nstat** | |
| | |
| **rtd** | |
| | |
| **rtp-streams** | |
| | |
| **rtp-analyse** | |
| | |
| **srt** | |
| | |
| **stat** | |
| | |

To be completed.

NB: Many of these taps produce a lot of data.

### Examples
```
{"jsonrpc":"2.0","id":1,"method":"tap", "params":{"tap0":"conv:Ethernet"}}
{"jsonrpc":"2.0","id":1,"result":{"taps":[{"tap":"conv:Ethernet","type":"conv","convs":[{"saddr":"VMware_d9:d3:b5","daddr":"VMware_fb:f9:13","rxf":1,"rxb":74,"txf":1,"txb":74,"start":0.000000000,"stop":0.000075000,"filter":"eth.addr==00:0c:29:d9:d3:b5 && eth.addr==00:0c:29:fb:f9:13"}],"proto":"Ethernet","geoip":false}]}}

{"jsonrpc":"2.0","id":1,"result":{"taps":[{"tap":"expert","type":"expert","details":[{"f":2,"s":"Chat","g":"Sequence","m":"Connection establish acknowledge (SYN+ACK): server port 80","p":"TCP"},{"f":1,"s":"Chat","g":"Sequence","m":"Connection establish request (SYN): server port 80","p":"TCP"}]}}

{"jsonrpc":"2.0","id":2,"method":"tap", "params":{"tap0":"seqa:tcp"}}
{"jsonrpc":"2.0","id":2,"result":{"taps":[{"tap":"seqa:tcp","type":"flow","nodes":["192.168.3.85","192.168.3.78"],"flows":[{"t":"0.000000","n":[0,1],"pn":[46815,80],"c":"Seq = 0"},{"t":"0.000075","n":[1,0],"pn":[80,46815],"c":"Seq = 0 Ack = 1"}]}]}}

{"jsonrpc":"2.0","id":3,"method":"tap", "params":{"tap0":"stat:http_req"}}
{"jsonrpc":"2.0","id":3,"result":{"taps":[{"tap":"stats:http_req","type":"stats","name":"HTTP/Requests","stats":[{"name":"HTTP Requests by HTTP Host","count":1,"rate":0.1431,"perc":100,"burstrate":0.0100,"bursttime":0.000,"sub":[{"name":"web01","count":1,"rate":0.1431,"perc":100.00,"burstrate":0.0100,"bursttime":0.000,"sub":[{"name":"/MyApp/Home/About","count":1,"rate":0.1431,"perc":100.00,"burstrate":0.0100,"bursttime":0.000}]}]}]}]}
```
---
