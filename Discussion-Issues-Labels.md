## Discussion page how we want to handle labels for issues (and MR):

### Proposal

It is obvious that we currently use labels primarily for issues.

For the future I propose to have this set of labels:

| Name | Description | How to set |
| --- | --- | --- |
| build|The tools and environment required to create Wireshark. This includes CMake, Visual Studio, gcc, clang, NSIS, and associated testing and packaging scripts.| manually, automatically |
| crash|Crash or hang in a Wireshark executable| manually, automatically |
| docs|Documentation| manually |
|enhancement|Enhancement or feature request| automatically via template |
| translations|Translation and localization in Wireshark| manually |
| web sites|The Wireshark web sites, including www.wireshark.org ask.wireshark.org, and gitlab.wireshark.org. Problems with infrastructure-related network services such as SMTP and DNS should be reported here as well.| manually |
| cli::dumpcap|Packet capture tool used by Wireshark, TShark, and sharkd| manually |
| cli::other|Other commmand line utilities that ship with Wireshark: editcap, mergecap, capinfos, text2pcap, etc.| manually |
| cli::tshark|The TShark text-mode analyzer| manually, automatically |
| ui::qt|The main Wireshark (Qt) UI| manually |
| lib::lua| LUA extension | manually |
| lib::wireshark|The library that dissects packets| manually |
| lib::wiretap|The library that reads and writes different capture file formats (libwiretap).| manually |
| lib::wsutil|The library that provides common utility functions (libwsutil).| manually |
| dfilter| Everything that has to do with display filters.|manually|
| os::linux|Linux Distributions like Debian, Ubuntu, Red Hat Linux, Fedora and others| manually |
| os::macos|macOS| manually |
| os::other|FreeBSD, NetBSD, OpenBSD, AIX, HP-UX, Solaris, and other OS| manually |
| os::windows|Microsoft Windows| manually |
| version::outdated|Version reached end of life | automatically |
| version::3.2|Version 3.2.x| manually, maybe automatically |
| version::3.4|Version 3.4.x| manually, maybe automatically |
| version::3.6|Version 3.6.x| manually, maybe automatically |
| version::dev|Development or prerelease versions| manually, automatically |
|ws-status::unconfirmed | This bug has recently been added to the issue tracker. Nobody has confirmed that this bug is valid. | automatically |
|ws-status::confirmed | This bug is valid. | manually, automatically |
|ws-status::waiting-for-response | To solve this issue additional data or information (a sample capture for example) is required. Most of the time the creator of the issue should add this data. | manually |
|ws-status::in-progress | This bug is not yet resolved, but is assigned to the proper person who is working on the bug. | automatically, manually |
|ws-status::fixed | This issue has been resolved | automatically |
|ws-status::closed | The issue is closed (maybe duplicate, won't fix or invalid)| automatically, manually |


The scoped label version has to be updated every time we introduce a new major version. All issues having a now end of life label have to be updated to version::outdated. This can easily be done with some API calls.

Setting the label requires manual interaction (atm). So yes, this label won't reflect the real state when the issue is closed automatically (for example when a MR referencing this issue is merged or when the issue is marked as an duplicate).

Furthermore a normal user is not allowed to set labels at the moment. Having the label in the issue template won't add the label when opening an issue.

There is bot available [3] which can handle most of the ws-status:: labels automatically.

### Current state

Gitlab has a feature called labels [1] to help organize, tag and keep track of issues and MR.
Labels with a double-colon (::) are so called "scoped labels" [2]. These are mutually exclusive.

With the migration from Bugzilla to Gitlab, all the labels of the follwing two tables were generated. These tables list also the current (state: 2021-04-26 ~18:20 UTC) usage of each label.

|Name |Description | Open Issues | Open MR |
| --- | --- | --- | --- |
|build|The tools and environment required to create Wireshark. This includes CMake, Visual Studio, gcc, clang, NSIS, and associated testing and packaging scripts.| 56 | 0 |
|crash|Crash or hang in a Wireshark executable| 34 | 0 |
|docs|Documentation| 27 | 0 |
|enhancement|Enhancement or feature request| 486 | 2 |
| incident|Denotes a disruption to IT services and the associated issues require immediate attention| 0 | 0 |
| question|General questions that are not bug or enhancement requests. These should be asked at https://ask.wireshark.org/| 1 | 0 |
| translations|Translation and localization in Wireshark| 1 | 0 |
| web sites|The Wireshark web sites, including www.wireshark.org ask.wireshark.org, and gitlab.wireshark.org. Problems with infrastructure-related network services such as SMTP and DNS should be reported here as well.| 26 | 0 |

| Name | Description | Open Issues | Open MR |
| --- | --- | --- | --- |
| cli::dumpcap|Packet capture tool used by Wireshark, TShark, and sharkd| 20 | 0 |
| cli::other|Other commmand line utilities that ship with Wireshark: editcap, mergecap, capinfos, text2pcap, etc.| 37 | 0| 
| cli::tshark|The TShark text-mode analyzer| 59 | 0 |
| lib::lua| 0 | 0 |
| lib::wireshark|The library that dissects packets| 482 | 1 |
| lib::wiretap|The library that reads and writes different capture file formats (libwiretap).| 56 | 0 |
| lib::wsutil|The library that provides common utility functions (libwsutil).| 18 | 0 |
| os::bsd|FreeBSD, NetBSD, OpenBSD, and other BSD Unices| 1 | 0 |
| os::debian|Debian| 9 | 0 |
| os::fedora|Fedora| 1 | 0 |
| os::linux|Other Linux Distributions| 71 | 0 |
| os::macos|macOS| 92 | 0 |
| os::red hat|Red Hat Linux| 5 | 0 |
| os::ubuntu|Ubuntu| 24 | 0 |
| os::unix|AIX, HP-UX, Solaris, and other Unices| 1 | 0 |
| os::windows|Microsoft Windows| 368 | 0 |
| ui::gtk|The old Wireshark (GTK+) UI| 1 | 0 |
| ui::qt|The main Wireshark (Qt) UI| 392 | 0 |
| version::0.x|Versions prior to 1.0.0| 9 | 0 |
| version::1.0|Version 1.0.x| 8 | 0 |
| version::1.10|Version 1.10.x| 34 | 0 |
| version::1.12|Version 1.12.x| 41 | 0 |
| version::1.2|Version 1.2.x| 13 | 0 |
| version::1.4|Version 1.4.x| 11 | 0 |
| version::1.6|Version 1.6.x| 13 | 0 |
| version::1.8|Version 1.8.x| 10 | 0 |
| version::2.0|Version 2.0.x| 51 | 0 |
| version::2.2|Version 2.2.x| 64 | 0 |
| version::2.4|Version 2.4.x| 67 | 0 |
| version::2.6|Version 2.6.x| 67 | 0 |
| version::3.0|Version 3.0.x| 109 | 0 |
| version::3.2|Version 3.2.x| 99 | 0 |
| version::3.4|Versions 3.4.x| 1 | 0 |
| version::dev|Development or prerelease versions| 363 | 0 |



[1]: https://docs.gitlab.com/ee/user/project/labels.html
[2]: https://docs.gitlab.com/ee/user/project/labels.html#scoped-labels
[3]: https://github.com/uhei/ws-issue-bot
