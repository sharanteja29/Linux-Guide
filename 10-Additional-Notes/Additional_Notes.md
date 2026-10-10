# Linux Disk and Storage Management — Cloud Engineer Notes

## 1. Introduction

Disk and storage management in Linux includes identifying storage devices, checking capacity, partitioning disks, creating filesystems, mounting filesystems, managing logical volumes, and understanding swap.

For a Cloud Engineer, the key is understanding how additional storage—such as an AWS EBS volume—becomes usable inside a Linux instance.

## 2. The Basic Storage Workflow

A common workflow for a new, empty disk is:

1. Identify the device.
2. Create a partition if needed.
3. Create a filesystem.
4. Create a mount-point directory.
5. Mount the filesystem.
6. Verify that it is accessible.
7. Configure a persistent mount if it must be mounted automatically after reboot.

Not every disk needs every step. A filesystem can sometimes be created directly on a whole device without a partition.

**Safety:** identify the correct device first. Partitioning and formatting can destroy existing data. Never copy a device name from an example without verifying it.

---

## 3. Viewing Disk Information

### `lsblk` — list block devices

```bash
lsblk
```

Shows block devices such as disks, partitions, and mount points.

For filesystem information:

```bash
lsblk -f
```

This can display filesystem types, labels, UUIDs, and mount points.

### `fdisk -l` — list partition information

```bash
sudo fdisk -l
```

Lists disks and partition information. Elevated privileges may be needed to see all details.

### `blkid` — inspect filesystem identifiers

```bash
sudo blkid
```

Shows information such as filesystem type and UUID. UUIDs are useful for identifying filesystems reliably in persistent mount configuration.

### `df -h` — check filesystem capacity

```bash
df -h
```

Displays filesystem size, used space, available space, and percentage used in human-readable units.

### `du -sh /path` — measure directory usage

```bash
du -sh /var/log
```

- `du`: reports disk usage.
- `-s`: summarizes the specified path.
- `-h`: uses human-readable units.

**Remember:** `df` reports filesystem-level capacity; `du` measures usage by files and directories. Their results can differ because of deleted-but-open files, filesystem metadata, or permissions.

---

## 4. Partition Management

A partition divides a disk into separately addressable regions. For example, `/dev/sdb` may have a partition named `/dev/sdb1`.

### `fdisk`

```bash
sudo fdisk /dev/sdb
```

Opens an interactive partitioning tool for the specified disk.

Common interactive actions include:

- `n`: create a partition.
- `p`: display the partition table in traditional `fdisk` workflows.
- `w`: write changes and exit.

The exact prompts depend on the disk's partition table and tool version. Verify the target disk before making changes.

### `parted`

```bash
sudo parted /dev/sdb
```

An alternative partitioning tool that supports GPT and other partition-table operations.

---

## 5. Filesystems and Formatting

A filesystem organizes storage into files and directories. A newly attached block device is not automatically ready for ordinary file storage.

### Create an ext4 filesystem

```bash
sudo mkfs.ext4 /dev/sdb1
```

### Create an XFS filesystem

```bash
sudo mkfs.xfs /dev/sdb1
```

These commands create filesystems on the specified partition.

**Critical warning:** `mkfs` can destroy existing data. Run it only on the verified, intended device or partition. Do not run example commands on your root disk or a disk containing data you need.

---

## 6. Mounting and Unmounting

### What is a mount point?

A mount point is a directory through which Linux accesses a filesystem. For example, `/data` can be the mount point for a separate disk.

### Create a mount point

```bash
sudo mkdir -p /data
```

### Mount a filesystem

```bash
sudo mount /dev/sdb1 /data
```

Makes the filesystem on `/dev/sdb1` accessible through `/data`. The filesystem must be compatible and ready to mount.

### Verify the mount

```bash
findmnt /data
df -h /data
lsblk
```

### Unmount a filesystem

```bash
sudo umount /data
```

Unmounting detaches the filesystem from `/data`. It does **not** erase data on the disk. After unmounting, the underlying directory on the parent filesystem becomes visible again. A filesystem may refuse to unmount if it is busy.

If it is busy, investigate processes using it:

```bash
sudo fuser -vm /data
```

Do not force-unmount casually, especially on production systems.

### Remount as read-write

```bash
sudo mount -o remount,rw /data
```

Attempts to remount an already mounted filesystem as read-write. This is not a universal fix for read-only filesystems.

### Mount the same filesystem at a different directory

After unmounting it from the original mount point:

```bash
sudo mkdir -p /mnt/mydisk
sudo mount /dev/sdb1 /mnt/mydisk
```

The files remain on the same filesystem. Changing the mount point does not move or delete them.

---

## 7. Persistent Mounts: `/etc/fstab` and UUIDs

A manual mount normally does not automatically persist after a reboot.

### Inspect UUIDs

```bash
sudo blkid
```

Or:

```bash
lsblk -f
```

A UUID identifies a filesystem. Using a UUID in persistent configuration can be more reliable than relying on a device name that may vary.

### `/etc/fstab`

`/etc/fstab` is a configuration file describing filesystems that Linux should mount, including mount points and options.

Illustrative entry:

```text
UUID=example-uuid  /data  ext4  defaults,nofail  0  2
```

Fields:

1. Filesystem identifier.
2. Mount point.
3. Filesystem type.
4. Mount options.
5. Legacy dump field.
6. Filesystem check order.

The UUID shown is a placeholder; never use it literally.

After carefully configuring a valid entry, test it:

```bash
sudo mount -a
```

Then verify:

```bash
findmnt /data
df -h /data
```

Incorrect `/etc/fstab` entries can cause mount or boot problems. Ensure the UUID, filesystem type, and mount point are correct, and test changes before rebooting. `nofail` may help a system boot if a nonessential volume is unavailable, but it does not make every storage failure harmless.

---

## 8. AWS EBS to Linux: Example Workflow

An EBS volume provides block storage to an EC2 instance. After attaching it in AWS, Linux must identify and prepare the device before using it as a filesystem.

Workflow for a **new, empty EBS volume**:

1. Create the EBS volume in AWS.
2. Attach it to the EC2 instance in the appropriate Availability Zone.
3. Identify the actual Linux device.
4. Create a partition if needed.
5. Create a filesystem.
6. Mount it at a directory.
7. Verify the mount.
8. Configure persistence if required.

Example commands, assuming the verified device is `/dev/nvme1n1` and you choose to put the filesystem directly on the whole device:

```bash
lsblk
sudo lsblk -f

# Destructive: only for a verified new, empty device
sudo mkfs.ext4 /dev/nvme1n1

sudo mkdir -p /tmp/logs
sudo mount /dev/nvme1n1 /tmp/logs

findmnt /tmp/logs
df -h /tmp/logs
```

Create a test log:

```bash
sudo touch /tmp/logs/application.log
echo "Application started" | sudo tee -a /tmp/logs/application.log
cat /tmp/logs/application.log
```

The Linux device name may differ from the device name selected in the AWS console. Always confirm the device using `lsblk` and related information before formatting.

**Note about `/tmp`:** it is intended for temporary files, and its contents may be cleaned up. For persistent production application logs, a location such as `/var/log/myapp` or `/data/logs` is generally more suitable.

---

## 9. LVM — Logical Volume Management

LVM adds a flexible storage-management layer between physical block devices and filesystems.

### The three layers

```text
Physical Volume (PV)
        ↓
Volume Group (VG)
        ↓
Logical Volume (LV)
        ↓
Filesystem
        ↓
Mount point
        ↓
Files
```

- **PV (Physical Volume):** a disk or partition prepared for LVM.
- **VG (Volume Group):** a pool of storage capacity made from one or more PVs.
- **LV (Logical Volume):** a logical block device allocated from a VG.

For example, two 10 GB PVs can contribute to a VG with approximately 20 GB of capacity before metadata and allocation considerations. Logical volumes can be allocated from that pool, with unused capacity available for later allocation.

### LVM commands to recognize

These examples are for a disposable lab using verified, unused storage. Do not run them blindly on system disks.

Prepare a physical volume:

```bash
sudo pvcreate /dev/sdb
```

Create a volume group:

```bash
sudo vgcreate vg_data /dev/sdb
```

Create a 10 GB logical volume:

```bash
sudo lvcreate -L 10G -n lv_logs vg_data
```

Create a filesystem on the logical volume:

```bash
sudo mkfs.ext4 /dev/vg_data/lv_logs
```

Mount it:

```bash
sudo mkdir -p /data/logs
sudo mount /dev/vg_data/lv_logs /data/logs
```

Inspect LVM:

```bash
sudo pvs
sudo vgs
sudo lvs
```

LVM is optional; a filesystem can be created directly on a disk or partition without LVM. For now, understand the layers and the purpose of the commands rather than mastering advanced resizing and recovery.

---

## 10. Swap Management

Swap is disk-backed space that Linux can use for selected memory pages when needed. It can help during memory pressure, but it is much slower than RAM and is not a replacement for adequate memory.

Swap can be configured on a partition or a swap file.

### Prepare swap

```bash
sudo mkswap /dev/sdb1
```

Prepares the specified device or partition for swap use. This can overwrite existing data or filesystem information on the target.

### Activate swap

```bash
sudo swapon /dev/sdb1
```

### Deactivate swap

```bash
sudo swapoff /dev/sdb1
```

Deactivating swap may require enough memory to move swapped pages back into RAM or elsewhere, so it can fail or create memory pressure.

### Inspect swap and memory

```bash
swapon --show
free -h
```

Persistent swap activation can also be configured through `/etc/fstab`.

**Safety:** never run `mkswap` on a device until you have verified it is intended for swap and contains no needed data. Avoid experimenting with active system swap on a real machine.

---

## 11. `df` vs `du`

| Command | What it measures |
|---|---|
| `df -h` | Capacity and available space on mounted filesystems |
| `du -sh /path` | Space used by files and directories under a path |

Example:

```bash
df -h
du -sh /var/log
du -sh /tmp
```

If a filesystem is nearly full, `df` helps identify which filesystem is affected, and `du` can help find large directories. Their results may differ because of deleted-but-open files, permissions, and filesystem metadata.

---

## 12. Cloud Engineer Priority

### Learn well

- `lsblk`, `lsblk -f`
- `df -h`, `du -sh`
- `blkid`
- Partition → filesystem → mount workflow
- `mount`, `umount`, `findmnt`
- Persistent mounting and the purpose of `/etc/fstab`
- Filesystem UUIDs
- Safe identification of disks before destructive operations

### Understand conceptually

- `fdisk` and `parted`
- ext4 vs XFS as filesystem choices
- PV → VG → LV in LVM
- `mkswap`, `swapon`, `swapoff`

### Defer until needed

- Advanced partition-table operations
- Complex LVM resizing, snapshots, and recovery
- Advanced swap tuning
- Filesystem repair and recovery procedures

---

## 13. Quick Revision Cheat Sheet

```text
VIEW STORAGE
lsblk                    → list block devices
lsblk -f                 → filesystems, UUIDs, mount points
sudo fdisk -l            → list partition details
sudo blkid               → filesystem UUIDs and types
df -h                    → filesystem capacity
du -sh /path             → directory usage

PARTITION / FORMAT
sudo fdisk /dev/sdb      → interactive partition management
sudo parted /dev/sdb     → alternative partitioning tool
sudo mkfs.ext4 /dev/sdb1 → create ext4 filesystem
sudo mkfs.xfs /dev/sdb1  → create XFS filesystem

MOUNT
sudo mkdir -p /data      → create mount point
sudo mount /dev/sdb1 /data → mount filesystem
findmnt /data            → inspect mount
sudo umount /data        → unmount filesystem
sudo mount -a            → attempt configured fstab mounts

PERSISTENCE
sudo blkid               → identify filesystem UUID
cat /etc/fstab           → inspect persistent mount configuration

LVM
pvcreate                 → prepare PV
vgcreate                 → create VG
lvcreate                 → create LV
pvs / vgs / lvs          → inspect LVM layers

SWAP
mkswap                   → prepare swap
swapon                   → activate swap
swapoff                  → deactivate swap
swapon --show            → inspect active swap
free -h                  → inspect memory and swap
```

## Final mental model

```text
Disk / EBS block device
        ↓
Partition (if needed)
        ↓
Filesystem (ext4 / XFS)
        ↓
Mount point
        ↓
Files and directories
```

With LVM, logical volumes add another layer before the filesystem:

```text
PV → VG → LV → Filesystem → Mount point → Files
```

For your current Cloud Engineer preparation, focus on understanding this workflow, identifying storage safely, checking capacity, mounting and unmounting, and knowing how persistent mounts work. Practise destructive operations only on a disposable VM or a confirmed empty test volume.
