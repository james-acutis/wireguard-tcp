# wireguard-tcp
This repository implements changes to the Linux kernel implementation of WireGuard to extend functionality and allow use of TCP as a transport protocol.

There are two patches and a tarball of the source code in this repo at present.

The wireguard_tcp_debug.diff is a diff that takes the clean linux kernel WireGuard module to a state with TCP implemented and verbose debugging code on. This patch works at present and has been tested to successfully implement a TCP version of WireGuard.

The wireguard_tcp_clean.diff is a diff that takes the clean linux kernel WireGuard module to a state with TCP implemented and verbose debugging code pulled out. This patch DOES NOT work at present. This will be fixed over the next few days.

The wireguard_tcp_kernel_code_debug.tgz is the source code after applying the wireguard_tcp_debug.diff.

The remaining files are documentating and design documents.
