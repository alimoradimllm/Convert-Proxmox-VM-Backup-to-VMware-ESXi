# Convert Proxmox Backups to VMware/ESXi

Easily convert Proxmox VM backups (`.vma.zst`) into VMware-compatible virtual disks (`.vmdk`) for use with VMware Workstation, Fusion, or ESXi.  
This guide provides a simple step-by-step process to extract, convert, and import your Proxmox virtual machines into a VMware environment.  

---

## 🚀 Prerequisites

Make sure the following tools are installed on your Linux system:

```bash
sudo apt update
sudo apt install zstd qemu-utils
```

---

## 🔄 Conversion Steps

1. **Place your backup file in the working directory**  
   Copy your Proxmox backup file (`your-backup.vma.zst`) to the folder where you’ll perform the conversion.

2. **Decompress the backup file**  
   Convert the compressed `.zst` file into a `.vma` archive:

   ```bash
   unzstd your-backup.vma.zst
   ```

   This produces a `.vma` file.

3. **Extract the `.vma` archive**  
   Use the `vma` tool to unpack the contents:

   ```bash
   vma extract your-backup.vma ./extracted
   ```

   This will create an `extracted` directory containing the VM disk(s), such as `disk-drive-scsi0.raw`.

4. **Convert RAW disk to VMDK**  
   Transform the Proxmox RAW disk into VMware’s `.vmdk` format:

   ```bash
   qemu-img convert -f raw -O vmdk ./extracted/disk-drive-scsi0.raw vm-disk.vmdk
   ```

5. **Import into VMware**  
   - Create a new VM in VMware Workstation, Fusion, or ESXi.  
   - Select **"Use an existing virtual disk"** and point it to `vm-disk.vmdk`.

---

## 📌 Notes

- Ensure the VM hardware configuration (CPUs, memory, disk controller type, etc.) in VMware matches the original Proxmox VM.  
- You can inspect disk details with:

  ```bash
  qemu-img info vm-disk.vmdk
  ```

- If your VM has multiple disks, repeat the conversion for each RAW disk file.

---

## 📜 License

This project is licensed under the [MIT License](LICENSE).  
