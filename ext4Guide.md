# 📘Renaming EXT4 Partition Labels & Auto-Mounting Drives on Arch Linux

This guide explains how to:

* 🏷️ Rename (label) EXT4 partitions
* 📌 Auto-mount a drive using `/etc/fstab`
* 🔐 Fix permissions so your user can access the drive

Perfect for organizing disks and making them mount consistently.

---

## 🔧 1. Check Your Partitions

Use `lsblk` to view all devices, labels, and UUIDs:

```bash
lsblk -f
```

You’ll see output similar to:

```
sda2  ext4   <no label>  495ace54-6124-43ae-999f-4053f17cefdf   /
sda3  ext4   <no label>  b8f5866f-0ee6-4611-af7d-c1d4c4535e65   /home
sdb1  ext4   Orbit       221cbde8-e8b3-4958-b9d2-0c09631c3af4   /home/xqf/Orbit
```

---

## 🏷️ 2. Rename an EXT4 Partition Label

### 📌 Renaming a non-root partition

Safe to do while the system is running.

Example: renaming `/dev/sda3` to `arch-home`:

```bash
sudo e2label /dev/sda3 arch-home
```

### ⚠️ Renaming the root (`/`) partition

The root filesystem is **locked while running**, so you must use a **Live USB**:

1. Boot into any Linux live environment
2. Open a terminal
3. Run:

```bash
sudo e2label /dev/sda2 arch-root
```

---

## 📂 3. Create a Mount Point

Before auto-mounting a drive, create its mount directory:

```bash
mkdir -p /home/xqf/Orbit
```

Replace the path if you want a different location.

---

## 📋 4. Add the Drive to `/etc/fstab`

1. Get the drive’s UUID:

   ```bash
   lsblk -f
   ```

2. Edit `/etc/fstab`:

   ```bash
   sudo nano /etc/fstab
   ```

3. Add a line like this (example UUID shown):

   ```fstab
   UUID=221cbde8-e8b3-4958-b9d2-0c09631c3af4   /home/xqf/Orbit   ext4   defaults,noatime   0 2
   ```

**Recommended options:**

* `defaults` → safe default behavior
* `noatime` → reduces disk writes, improves SSD longevity

Save and exit: **Ctrl + O**, **Enter**, **Ctrl + X**

---

## 🔄 5. Apply the New Mount

Reload all fstab entries:

```bash
sudo mount -a
```

Verify the mount worked:

```bash
lsblk -f
```

If there are no errors, the drive will auto-mount on every boot.

---

## 🔐 6. Fix Drive Ownership

To ensure your user (xqf) can read and write:

```bash
sudo chown -R xqf:xqf /home/xqf/Orbit
```

Check permissions:

```bash
ls -ld /home/xqf/Orbit
```

Expected:

```
drwxr-xr-x  ...  xqf xqf  /home/xqf/Orbit
```

---

## 🎉 Done!

You successfully:

* ✔ Renamed EXT4 filesystem labels
* ✔ Added a permanent mount to `/etc/fstab`
* ✔ Set correct permissions for user access
