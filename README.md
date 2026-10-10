# Linux Guide — Linux, Networking & Cloud Fundamentals

A hands-on learning repository documenting my journey through **Linux fundamentals** as preparation for **Cloud Engineering**. It contains chapter-based notes, commands, explanations, and practical concepts that I am learning and revising along the way.

The repository is organised so that learners can move from Linux basics toward the system-administration knowledge used when working with servers and cloud infrastructure.

## 🎯 Goals

- Build a strong foundation in Linux and the command line.
- Understand Linux directories, users, groups, files, and permissions.
- Learn to inspect and manage processes.
- Monitor system resources and understand basic troubleshooting.
- Understand disks, partitions, filesystems, mounts, swap, and LVM.
- Practise safely in a Linux lab environment.
- Prepare for the next stages of learning: networking, shell scripting, and cloud engineering.

## 📚 Repository Structure

The repository currently contains these nine chapters:

| Chapter | Topic | What it covers |
|---|---|---|
| [01 — Getting Started](./01-getting-started/) | Linux fundamentals and setup | Linux fundamentals, Linux structure, distributions, setting up Linux with Docker on Windows, and package managers. |
| [02 — Folder Structure](./02-folder-structure/) | Linux filesystem | Important system directories and where files and configuration belong. |
| [03 — User Management](./03-user-management/) | Users and groups | Linux accounts, groups, and basic user administration. |
| [04 — File Management](./04-file-management/) | Files and directories | Navigating, creating, viewing, copying, moving, and removing files and directories. |
| [05 — Vim Shortcuts](./05-vim-shortcuts/) | Text editing | Vim/vi modes, navigation, and essential editing shortcuts. |
| [06 — File Permissions](./06-file-permissions/) | Access control | Read, write, and execute permissions, ownership, and permission management. |
| [07 — Process Management](./07-process-management/) | Running processes | Inspecting and managing processes and understanding process behaviour. |
| [08 — Monitoring](./08-monitoring/) | System resources | Monitoring system usage and identifying potential resource issues. |
| [09 — Disk Management](./09-disk-management/) | Storage | Block devices, partitions, filesystems, formatting, mounting/unmounting, LVM, swap, and storage inspection. |

### Chapter 01 — Getting Started files

- [Linux Fundamentals](./01-getting-started/01-fundamentals.md)
- [Linux Structure and Setup Notes](./01-getting-started/02-linux-structure.md)
- [Linux Distributions](./01-getting-started/03-linux-distributions.md)
- [Linux & Docker Setup](./01-getting-started/04-setup.md)
- [Package Managers](./01-getting-started/05-package-manager.md)

Each chapter folder contains its own notes. Open the folder to explore the material for that topic.

## 🚀 How to Use This Repository

1. Start with **Chapter 01** and progress through the chapters in order.
2. Read the explanation for each concept before trying it in the terminal.
3. Practise the commands in a dedicated lab environment.
4. Verify the output and make sure you understand what changed.
5. When something fails, investigate the error and work out why it happened.
6. Revisit earlier chapters to reinforce commands and concepts used later.

The aim is to understand Linux well enough to perform basic administration and troubleshoot common problems—not simply memorise commands.

## 💻 Practice Environment

The setup notes in Chapter 01 cover using **Docker Desktop on Windows with an Ubuntu container**. This makes it possible to practise many Linux command-line concepts without replacing the host operating system.

Keep in mind that a container is not the same as a full Linux virtual machine. Some activities involving boot behaviour, system services, hardware, partitions, or real disk devices may require a full VM or a dedicated cloud VM. Use a disposable lab for experiments that could modify storage or system configuration.

## 🧭 Learning Roadmap

The planned learning sequence is:

1. **Linux fundamentals** — the chapters currently organised in this repository.
2. **Networking fundamentals** — IP addresses, ports, DNS, TCP/UDP, routing, and firewalls/security groups.
3. **Shell scripting** — automate tasks after building the Linux and networking foundation.
4. **Cloud engineering** — continue into cloud platforms and related infrastructure tools.

Networking and shell scripting are future stages of the roadmap; they are not listed as completed chapters in the current nine-chapter structure.

## 🛡️ Practice Safely

- Practise administrative tasks in a lab rather than on important systems.
- Be especially careful with deletion, ownership and permission changes, partitioning, formatting, and mount configuration.
- Understand the purpose and impact of an operation before running it.
- Verify the result after making a change.
- Use elevated privileges only when necessary.

## 🤝 Contributions and Corrections

This is a learning project. Suggestions, corrections, and improvements are welcome. When updating notes, aim to keep them clear, practical, and useful to someone learning Linux for the first time.

## 📌 Learning Philosophy

**Learn → Practise → Troubleshoot → Verify → Revise**

Mistakes in a safe lab are part of the learning process. Understanding why a command works—and how to verify its result—is more valuable than memorising syntax alone.

---

**Repository:** [sharanteja29/Linux-Guide](https://github.com/sharanteja29/Linux-Guide)
