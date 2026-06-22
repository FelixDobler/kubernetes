# Prerequisites

- install zfs (linux headers and dkms)

# Proxmox Disk Passthrough

For SATA Disks:
```
qm set VM_ID -scsi1 /dev/disk/by-id/DEVICE_IDENTIFIER
qm set VM_ID -scsi2 /dev/disk/by-id/DEVICE_IDENTIFIER
```

Otherwise NVME Passthrough if iommu is available

# Create pool

```
zpool create -f zfspv-pool -o ashift=12 -O compression=lz4 -O atime=off mirror /dev/sdb /dev/sdc

```
