# Linux & Docker Setup

## 1. Overview

Using **Docker Desktop on Windows** to practice Linux with Ubuntu.

```text
Windows
   ↓
Docker Desktop
   ↓
Docker Engine
   ↓
Ubuntu Container
```

This provides a Linux environment without replacing Windows or installing a full VM.

---

## 2. Docker Basics

### Image

An **image** is a template used to create containers.

```text
ubuntu:latest
```

### Container

A **container** is a running instance of an image.

```text
ubuntu-container
```

### Container vs VM

- **Container:** shares the underlying kernel.
- **VM:** runs its own guest OS and kernel.

---

## 3. Current Setup

| Setting | Value |
|---|---|
| Host OS | Windows |
| Docker | Docker Desktop |
| Image | `ubuntu:latest` |
| Container | `ubuntu-container` |
| Hostname | `ubuntu-dev` |
| CPU Limit | 2 CPUs |
| Memory | 4 GB |
| Timezone | `Asia/Kolkata` |
| Shared Folder | `C:\Users\shara\Downloads\ubuntu-container` |
| Container Folder | `/data` |
| Ports | `2222 → 22`, `8080 → 80` |

---

## 4. Create Container

```powershell
docker run -dit `
  --name ubuntu-container `
  --hostname ubuntu-dev `
  --restart unless-stopped `
  --cpus="2" `
  --memory="4g" `
  --mount type=bind,source="C:/Users/shara/Downloads/ubuntu-container",target=/data `
  -p 2222:22 `
  -p 8080:80 `
  --env TZ=Asia/Kolkata `
  --env LANG=en_US.UTF-8 `
  ubuntu:latest /bin/bash
```

### Important Options

| Option | Purpose |
|---|---|
| `-d` | Run in background |
| `-it` | Interactive terminal |
| `--name` | Container name |
| `--hostname` | Linux hostname |
| `--cpus` | CPU limit |
| `--memory` | Memory limit |
| `--mount` | Windows folder → container |
| `-p` | Port mapping |
| `--env` | Environment variable |

---

## 5. Bind Mount

Windows folder:

```text
C:\Users\shara\Downloads\ubuntu-container
```

Inside Ubuntu:

```text
/data
```

```text
Windows Folder
      ↕
   Bind Mount
      ↕
Container /data
```

Example:

```bash
cd /data
touch test.txt
```

The file will also appear in the Windows folder.

---

## 6. Port Mapping

Format:

```text
HOST_PORT:CONTAINER_PORT
```

Current mappings:

```text
2222 → 22
8080 → 80
```

- `22` → commonly used for SSH
- `80` → commonly used for HTTP

Mapping a port does not automatically start a service.

---

## 7. Enter Ubuntu

From PowerShell:

```powershell
docker exec -it ubuntu-container /bin/bash
```

Inside the container:

```text
root@ubuntu-dev:/#
```

- `root` → current user
- `ubuntu-dev` → hostname
- `/` → current directory

Exit:

```bash
exit
```

---

## 8. Useful Docker Commands

```powershell
docker ps
docker ps -a
docker images

docker start ubuntu-container
docker stop ubuntu-container
docker restart ubuntu-container

docker logs ubuntu-container
docker inspect ubuntu-container

docker exec -it ubuntu-container /bin/bash
```

---

## 9. Useful Linux Commands

```bash
pwd                  # Current directory
ls -la               # List files
cd /data             # Shared folder

whoami               # Current user
hostname             # Hostname
uname -a             # System information
cat /etc/os-release  # OS information

df -h                # Disk usage
free -h              # Memory usage
ps                   # Processes
```

---

## 10. Important Linux Directories

```text
/       → Root
/root   → Root user's home
/home   → User home directories
/etc    → Configuration
/usr    → Programs and libraries
/var    → Logs and variable data
/tmp    → Temporary files
/dev    → Device files
/proc   → Process/kernel information
/sys     → Kernel/device information
/data   → Windows bind-mounted folder
```

---

## 11. Windows vs Linux

### Windows

```text
PS C:\Users\shara>
```

### Ubuntu Container

```text
root@ubuntu-dev:/#
```

Enter Ubuntu:

```powershell
docker exec -it ubuntu-container /bin/bash
```

Exit:

```bash
exit
```

---

## 12. Mental Model

```text
Windows
   ↓
Docker Desktop
   ↓
Docker Engine
   ↓
ubuntu:latest
   ↓
ubuntu-container
   ├── Linux filesystem
   ├── /data → Windows folder
   └── 2222→22 | 8080→80
```

### Learning Goals

- Linux commands
- Linux filesystem
- Users & permissions
- Processes
- Networking
- Shell scripting
- Docker
- Linux troubleshooting