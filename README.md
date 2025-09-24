## 🧠 WizardCloud Partition-Aware Backup (Partclone + PV)

Efficiently clone only used blocks of the Linux root partition, skipping empty space for faster backups.

```bash
# Mount Windows Share
sudo mkdir -p /mnt/winshare
sudo mount -t cifs //192.168.40.50/Public /mnt/winshare -o guest,vers=3.0

# Install partclone if needed
sudo apt update
sudo apt install partclone

# Backup Linux root partition only (mmcblk0p2)
sudo partclone.ext4 -c -s /dev/mmcblk0p2 | pv | gzip > /mnt/winshare/WizardCloud_root.img.gz

# Verify Output
ls -lh /mnt/winshare/WizardCloud_root.img.gz

# Inspect Image Header
gunzip -c /mnt/winshare/WizardCloud_root.img.gz | file -

# Restore (Optional)
gunzip -c /mnt/winshare/WizardCloud_root.img.gz | sudo partclone.restore -s - -o /dev/mmcblk0p2

# Archive with Timestamp
mv /mnt/winshare/WizardCloud_root.img.gz /mnt/winshare/WizardCloud_root_2025-09-25.img.gz
```

> Captures only used blocks of the root partition for a faster, leaner backup. Ideal for restore, versioning, and automation.
