# wireguard-tcp
This repository implements changes to the Linux kernel implementation of WireGuard to extend functionality and allow use of TCP as a transport protocol.

There are three patches and a tarball of the source code in this repo at present.

The wireguard_tcp_debug.diff is a diff that takes the clean linux kernel WireGuard module to a state with TCP implemented and verbose debugging code on. This patch works at present and has been tested to successfully implement a TCP version of WireGuard.

The wireguard_tcp_clean.diff is a diff that takes the clean linux kernel WireGuard module to a state with TCP implemented and verbose debugging code pulled out. This patch DOES NOT work at present. This will be fixed over the next few days.

The wireguard_tcp_kernel_code_debug.tgz is the source code after applying the wireguard_tcp_debug.diff.

The remaining files are documentating and design documents.

## Kernel
Building the WireGuardTCP wireguard kernel module requires several steps:
 - Add the "wireguard.h" include file that adds the modified netlink message for transport mode to the kernel by copying it to your build tree at ./include/uapi/linux/wireguard.h
 - Backup the existing /usr/include/linux/wireguard.h file and the "wireguard.h" include file in this repository in its place so you may build the updated wireguard userland tool.
 - Rebuild and reinstall a new kernel
 - Recompile the wireguard module using the command "make M=drivers/net/wireguard modules"
- Reinstall the wireguard module using the command "make M=drivers/net/wireguard modules_install"

 ## Userland
 - Create a directory and cd into that directory
 - Obtain the Wireguard userland sources from git
   - git clone https://git.zx2c4.com/wireguard-tools
 - cd into the parent directory you clined the wireguard-tools from
 - Download the wireguard-tools.diff patch from this repository into the parent directory from the previous step
 - Patch the wireguard-tools using the command "patch -p1 < wireguard-tools.tgz"
 - cd into the wireguard-tools/src directory
 - Build the wireguard userland using the command "make"

## Special notes for using Wireguard in TCP mode  
 - After creating a wireguard device, you must decrese its MTU. Try a value around 1100 bytes for starters creating 
 - When configuring Wireguard for TCP
   - First add the interface
   - Then give it an IP address
   - Then set the MTU of the interface
   - Add the Wireguard private key
   - Configure Wireguard to use TCP using the command "wg set wg0 transport tcp"
   - Then bring up the interface
 - All other configuration can proceed as the original variant.
