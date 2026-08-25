# Created on Tuesday, 25 Aug 2026

# Hardware

## Server

| Component | Specification 
|---|---|
| Platform | Raspberry Pi 5 Model B Rev 1.1 |
| Storage interface | Raspberry Pi M.2 HAT+ |
| Primary storage | 512 GB-class NVMe SSD |
| Architecture | ARM64 |
| Boot storage | NVMe |

## Storage

The server is currently booting and operating from the NVMe SSD.

The operating system reports the NVMe device as:

`/dev/nvme0n1`

The physical storage device is partitioned as:

- `/dev/nvme0n1p1` — 512 MiB, VFAT, mounted at `/boot/firmware`
- `/dev/nvme0n1p2` — approximately 476 GiB, ext4, mounted at `/`

## Notes

The system currently has no microSD storage device mounted or visible as a block device.

Linux also reports `loop0` and `zram0` swap devices. These are virtual/compressed-memory swap devices and are not physical storage drives.