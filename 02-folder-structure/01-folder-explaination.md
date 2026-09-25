# Linux Folder Structure

Linux follows a **hierarchical filesystem**. Everything starts from the root directory `/`.

## 1. Linux Filesystem Tree

```text
/
├── bin
├── boot
├── dev
├── etc
├── home
├── lib
├── media
├── mnt
├── opt
├── proc
├── root
├── run
├── sbin
├── srv
├── sys
├── tmp
├── usr
└── var
```

The most important directories to remember are:

```text
/       → Root of the filesystem
/home   → Normal users
/root   → Root user's home
/etc    → Configuration files
/usr    → Applications and libraries
/var    → Variable data and logs
/tmp    → Temporary files
/dev    → Devices
/proc   → Process/kernel information
/sys    → Hardware/kernel information
/boot   → Boot files
```

---

# 2. `/` — Root Directory

`/` is the **top-level directory** of Linux.

Every other directory exists under `/`.

```text
/
├── home
├── etc
├── usr
├── var
└── ...
```

Check it:

```bash
pwd
```

Example:

```text
/
```

---

# 3. `/home` — User Home Directories

`/home` contains the home directories of normal users.

Example:

```text
/home
└── sharan
```

A user's personal files are usually stored here.

```bash
cd /home
ls
```

Typical contents:

```text
Documents
Downloads
Pictures
Videos
```

---

# 4. `/root` — Root User's Home

`/root` is the home directory of the **root user**.

It is different from `/`.

```text
/       → Root of filesystem
/root   → Home directory of root user
```

Check:

```bash
cd /root
pwd
```

---

# 5. `/etc` — Configuration

`/etc` contains system and application configuration files.

Examples:

```text
/etc/passwd
/etc/hosts
/etc/hostname
/etc/ssh/
```

Common commands:

```bash
ls /etc
cat /etc/hostname
cat /etc/hosts
```

Think:

```text
/etc = Configuration
```

---

# 6. `/usr` — User Programs and Data

`/usr` contains many applications, libraries and utilities used by the system.

Important directories include:

```text
/usr/bin
/usr/sbin
/usr/lib
/usr/share
```

For example:

```bash
ls /usr/bin
```

Many commands available in Linux are stored under `/usr/bin`.

> On modern Linux distributions, `/bin` and `/sbin` may be symbolic links to locations under `/usr`.

---

# 7. `/var` — Variable Data

`/var` contains data that changes frequently while the system is running.

Examples:

```text
/var/log
/var/cache
/var/lib
```

Logs are commonly stored in:

```bash
/var/log
```

Example:

```bash
ls /var/log
```

Think:

```text
/var = Changing data
```

---

# 8. `/tmp` — Temporary Files

`/tmp` is used for temporary files.

Example:

```bash
cd /tmp
touch test.txt
ls
```

Temporary files may be removed automatically depending on the system configuration.

Think:

```text
/tmp = Temporary
```

---

# 9. `/dev` — Devices

`/dev` contains special files representing devices.

Examples:

```text
/dev/sda
/dev/null
/dev/random
```

`/dev/null` is commonly used to discard output.

Example:

```bash
echo "hello" > /dev/null
```

---

# 10. `/proc` — Processes and Kernel Information

`/proc` is a **virtual filesystem**.

It provides information about:

- Running processes
- CPU
- Memory
- Kernel
- System configuration

Example:

```bash
ls /proc
```

Check CPU information:

```bash
cat /proc/cpuinfo
```

Check memory:

```bash
cat /proc/meminfo
```

Think:

```text
/proc = Process + kernel information
```

---

# 11. `/sys` — Kernel and Hardware Information

`/sys` is another virtual filesystem.

It provides information and interfaces related to:

- Hardware
- Devices
- Kernel
- Drivers

Example:

```bash
ls /sys
```

Think:

```text
/sys = System + hardware information
```

---

# 12. `/boot` — Boot Files

`/boot` contains files required during the Linux boot process.

Examples can include:

```text
Linux kernel
initramfs
bootloader files
```

Check:

```bash
ls /boot
```

---

# 13. `/bin` — Essential Commands

`/bin` contains essential executable commands.

Examples may include:

```text
ls
cp
mv
rm
cat
```

On modern Linux systems, `/bin` is often a symbolic link to `/usr/bin`.

---

# 14. `/sbin` — System Administration Commands

`/sbin` contains system administration utilities.

These commands are mainly used for system management.

On modern distributions, `/sbin` may also be linked into `/usr`.

---

# 15. `/lib` — Libraries

`/lib` contains important libraries required by programs and the Linux system.

Modern distributions may use:

```text
/usr/lib
```

instead of a separate traditional `/lib` layout.

---

# 16. `/media` — Removable Media

Used for automatically mounted removable devices.

Examples:

```text
USB drives
CD/DVDs
```

---

# 17. `/mnt` — Temporary Mount Point

`/mnt` is commonly used for temporarily mounting filesystems.

Example:

```bash
mount /dev/sdb1 /mnt
```

---

# 18. `/opt` — Optional Software

`/opt` is commonly used for optional or third-party software.

Example:

```text
/opt/application
```

---

# 19. `/srv` — Service Data

`/srv` is intended for data provided by system services.

For example, a server application may store service-related data here.

---

# 20. Important Directories at a Glance

| Directory | Purpose |
|---|---|
| `/` | Root of filesystem |
| `/home` | Normal users' home directories |
| `/root` | Root user's home |
| `/etc` | Configuration |
| `/usr` | Programs, libraries and shared data |
| `/var` | Logs and changing data |
| `/tmp` | Temporary files |
| `/dev` | Device files |
| `/proc` | Process/kernel information |
| `/sys` | Hardware/kernel information |
| `/boot` | Boot files |
| `/bin` | Essential commands |
| `/sbin` | System administration commands |
| `/lib` | Libraries |
| `/media` | Removable media |
| `/mnt` | Temporary mount point |
| `/opt` | Optional software |
| `/srv` | Service data |

---

# 21. Useful Commands for Exploring

Start at the root:

```bash
cd /
```

List directories:

```bash
ls
```

Detailed listing:

```bash
ls -ltr
```

Show current directory:

```bash
pwd
```

Move between directories:

```bash
cd /etc
cd /home
cd /var
cd /tmp
```

Go back:

```bash
cd ..
```

Go to home:

```bash
cd ~
```

---

# 22. Easy Way to Remember

```text
/       → Everything
/home   → Users
/root   → Root user
/etc    → Configuration
/usr    → Programs
/var    → Changing data/logs
/tmp    → Temporary
/dev    → Devices
/proc   → Processes
/sys    → System/hardware
/boot   → Boot
```

### Key Concept

Linux follows a **single hierarchical filesystem starting at `/`**.

```text
/
├── home      → Users
├── etc       → Configuration
├── usr       → Programs
├── var       → Logs/data
├── tmp       → Temporary
├── dev       → Devices
├── proc      → Processes
├── sys       → Hardware/kernel
└── boot      → Boot files
```