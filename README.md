# wireguard-tcp
This repository implements changes to the Linux kernel implementation of WireGuard to extend functionality and allow use of TCP as a transport protocol.

There are three patches and a tarball of the source code in this repo at present.

The wireguard_tcp_debug.diff is a diff that takes the clean linux kernel WireGuard module to a state with TCP implemented and verbose debugging code on. This patch works at present and has been tested to successfully implement a TCP version of WireGuard.

The wireguard_tcp_clean.diff is a diff that takes the clean linux kernel WireGuard module to a state with TCP implemented and verbose debugging code pulled out. This works at present and has been tested to successfully implement a TCP version of WireGuard.

The wireguard_tcp_kernel_code_debug.tgz is the source code after applying the wireguard_tcp_debug.diff.

The remaining files are documentating and design documents.

## Kernel
 - Obtain kernel sources for your Linux distribution.
   - This implementation of WireGuard over TCP was developed using Ubuntu and instructions for obtaining kernel sources and building them can be found here: https://wiki.ubuntu.com/Kernel/BuildYourOwnKernel
 - Download the wireguard_tcp_clean.diff from this repository into the top-level directory containing the Linux kernel source code
 - Patch the Linux kernel sources from within the top-level directory using the command "patch -p2 < wireguard_tcp_clean.diff"
 - Rebuild and reinstall a new kernel
 - Recompile the wireguard module using the command "make M=drivers/net/wireguard modules"
 - Reinstall the wireguard module using the command "sudo make M=drivers/net/wireguard modules_install"

## Userland
 - Backup the existing /usr/include/linux/wireguard.h file and overwrite it with the "wireguard.h" include file in this repository so you may build the updated wireguard userland tool.
 - Create a directory and cd into that directory
 - Obtain the Wireguard userland sources from git
   - git clone https://git.zx2c4.com/wireguard-tools
 - cd into the directory you cloned the wireguard-tools into (the parent directory of wireguard-tools)
 - Download the wireguard-tools.diff patch from this repository into the parent directory of the cloned wireguard-tools repository
 - Patch the wireguard-tools using the command "patch -p1 < wireguard-tools.diff"
 - cd into the wireguard-tools/src directory
 - Build the wireguard userland using the command "make"

## Special notes for using Wireguard in TCP mode  
 - After creating a wireguard device, you must specify a smaller MTU. Try a value around 1100 bytes to begin with 
 - When configuring Wireguard for TCP
   - First add the interface
   - Then give it an IP address
   - Then set the MTU of the interface
   - Add the Wireguard private key
   - Configure Wireguard to use TCP using the command "wg set wg0 transport tcp"
   - Then bring up the interface
 - All other configuration can proceed as the original variant.
