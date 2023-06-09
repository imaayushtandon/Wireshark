# Development/Security

This page is about the development to advance Wiresharks security. The related [Security](/Security) page contains more general security aspects like when and how a user might be affected.

There are many ways to improve Wireshark's security situation. Fortunately, none of them are mutually exclusive.

[[_TOC_]]

## Automatic Code Generation

It is possible (for some dissectors at least) to use a higher-level language to generate the C code for a dissector. If the translators have been reviewed to make sure they generate "safe" code, and the code they call has been reviewed to make sure it's safe, then dissectors written in those higher-level languages will be safe.

For example, some dissectors for protocols using ASN.1 are generated from an ASN.1 description of the protocol.

*Status:* We're already doing this for many ASN.1-based protocols as well as NCP, DCE/RPC BUTC, and others. Most (possibly all) of Wireshark's dissectors could be autogenerated.

## Code Review

The Wireshark development process includes patches from many different developers (with different levels of programming skills) all around the world, and only a few developers are doing the job of reviewing the patches before they are checked in the main source tree. This development model won't change for several reasons.

*Status:* Independent researchers have done informal reviews in the past. Unfortunately no comprehensive review has been done to date.

## Privilege Separation

We can reduce the potential for harm by running Wireshark as a non-privileged user. This is difficult on some platforms, since "root" or "Administrator" privileges may be needed for capture. See [CaptureSetup/CapturePrivileges](/CaptureSetup/CapturePrivileges) for details.

*Status:* Privilege separation was implemented in Wireshark 1.0. Only the 'dumpcap' utility needs to be run as root. See [Development/PrivilegeSeparation](/Development/PrivilegeSeparation) for more details.

## API Improvements

Epan's tvbuff routines have gone a long way in preventing security holes in Wireshark's core. What happens after a dissector pulls data **out** of a tvbuff is another story, and is where most of our serious bugs come from. It should be possible to extend Wireshark's API to make it easier to handle data safely.

*Status:* Unknown.

## Automated Testing

We have an [~~automated build process~~](http://buildbot.wireshark.org) [GitLab CI?](https://gitlab.com/wireshark/gitlab-migration/-/wikis/Automated-Builds-And-Continuous-Integration-(Buildbot)) that runs tshark against randpkt output, a capture file menagerie, and the menagerie after it's been fuzzed. Many bugs have been found using this process so far. More tests could be added, such as [Valgrind](http://valgrind.org/) and [Gimpel Lint](http://www.gimpel.com/).

It's also possible for developers to do fuzz/random testing themselves. Details can be found on the [FuzzTesting](/FuzzTesting) page.

*Status:* The Linux 64-bit buildbot runs fuzz tests after each checkin.

## Avoid problematic functions

We should avoid functions known to frequently cause security related problems, e.g. the ANSI-C function strcpy() will overwrite the destination area if the source string is too large for it.

*Status:* Work in progress, details can be found in [Development/SecureProgramming](/Development/SecureProgramming).

## A "Stable" Branch

We created a stable branch in SVN, and only allow bug fixes to be checked in to that branch. New development remains in the trunk. See [Development/ReleaseNumbers](/Development/ReleaseNumbers) for more info.

---

Imported from https://wiki.wireshark.org/Development/Security on 2020-08-11 23:13:05 UTC
