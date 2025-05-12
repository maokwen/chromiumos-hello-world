## OS

First problem is: My working labtop with Linux installed left in company, currently I only have a Windows PC in hand.
.
- Qemu on Windows host: not support virtio.
- Qemu with WSL2: kvm acceleration not supported.
- Do a flush new Linux installation on host.

I choose to install ArchLinux. For future use concidered, I was not using Debian or Ubuntu.

## Proxy

For proxy my network traffic, I use sing-box program with its tun mode. Not using the packet forwading on the gateway side.
