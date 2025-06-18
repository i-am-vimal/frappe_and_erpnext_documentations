# Moving Frappe Public/Private Files to a Different Disk Using Symlinks

This guide explains how to move Frappe file storage (`public/files` and `private/files`) to another partition or mounted disk using symbolic links.

## Example Paths

- **Original paths:**
  - Public: `/home/frappe/frappe-bench/sites/site1.local/public/files`
  - Private: `/home/frappe/frappe-bench/sites/site1.local/private/files`

- **New mount location:**
  - `/mnt/volume_blr1_02`

---

## 🚀 Steps

### Step 1: Stop Frappe processes
```bash
sudo supervisorctl stop all
```

### Step 2: Create new directories on the mounted disk
```bash
sudo mkdir -p /mnt/volume_blr1_02/public_files
sudo mkdir -p /mnt/volume_blr1_02/private_files
```

### Step 3: Sync existing files to the new location
```bash
sudo rsync -a /home/frappe/frappe-bench/sites/site1.local/public/files/ /mnt/volume_blr1_02/public_files/
sudo rsync -a /home/frappe/frappe-bench/sites/site1.local/private/files/ /mnt/volume_blr1_02/private_files/
```

### Step 4: Remove original folders
```bash
sudo rm -rf /home/frappe/frappe-bench/sites/site1.local/public/files
sudo rm -rf /home/frappe/frappe-bench/sites/site1.local/private/files
```

### Step 5: Create symbolic links to the new location
```bash
sudo ln -s /mnt/volume_blr1_02/public_files /home/frappe/frappe-bench/sites/site1.local/public/files
sudo ln -s /mnt/volume_blr1_02/private_files /home/frappe/frappe-bench/sites/site1.local/private/files
```

### Step 6: Verify symlinks
```bash
ls -l /home/frappe/frappe-bench/sites/site1.local/public/files
ls -l /home/frappe/frappe-bench/sites/site1.local/private/files
```

### Step 7: Start Frappe services
```bash
sudo supervisorctl start all
```

✅ **Done!** Your files are now served from the new disk using symbolic links.
