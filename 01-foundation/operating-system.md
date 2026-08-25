# Created on Tuesday, 25 Aug 2026

# Operating System

## Current Installation

| Item | Verified Value |
|---|---|
| Operating system | Debian GNU/Linux 13 (trixie) |
| Debian version | 13.6 |
| Architecture | arm64 |
| Kernel | 6.18.39+rpt-rpi-2712 |
| Kernel build | 29 Jul 2026 |

## Boot

The system is currently booting from the NVMe storage device.

The root filesystem is:

`/dev/nvme0n1p2`

The firmware partition is:

`/dev/nvme0n1p1`

mounted at:

`/boot/firmware`

## Verification

The operating system information was obtained directly from the running server using:

- `hostnamectl`
- `/etc/os-release`
- `uname -a`

The storage and mount configuration was verified using:

- `lsblk`
- `df -h`

## Security Note

Machine IDs, boot IDs, serial numbers, UUIDs, usernames, IP addresses and other identifying values are intentionally excluded from this public documentation.