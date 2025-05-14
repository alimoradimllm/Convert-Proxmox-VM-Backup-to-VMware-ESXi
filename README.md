# Convert Proxmox VM Backup to VMware/ESXi

This repository provides a step-by-step guide to convert a Proxmox VM backup (`.vma.zst`) into a VMware-compatible `.vmdk` disk image.

## Prerequisites

Install the required tools:

```bash
sudo apt update
sudo apt install zstd qemu-utils
```

## Steps to Convert

1. **Place your Proxmox backup file (`your-backup.vma.zst`) in your working directory.**

2. **Decompress the backup file:**

```bash
unzstd your-backup.vma.zst
```

This will produce a `.vma` file.

3. **Extract the contents of the `.vma` file:**

```bash
vma extract your-backup.vma ./extracted
```

This will create an `extracted` directory containing disk files (e.g., `disk-drive-scsi0.raw`).

4. **Convert the RAW disk to VMDK format:**

```bash
qemu-img convert -f raw -O vmdk ./extracted/disk-drive-scsi0.raw vm-disk.vmdk
```

5. **Import the `vm-disk.vmdk` into VMware Workstation or ESXi.**

   - Create a new VM in VMware and select **"Use an existing virtual disk"**.
   - Point it to `vm-disk.vmdk`.

## Notes

- Ensure the VM hardware configuration (e.g., number of CPUs, RAM, disk controller type) in VMware matches the original Proxmox VM settings.
- You can use `qemu-img info <filename>` to inspect disk format and details if needed.

---

## License

This project is licensed under the [MIT License](LICENSE).
