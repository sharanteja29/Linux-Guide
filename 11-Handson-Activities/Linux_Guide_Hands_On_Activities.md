# Linux Guide — Hands-On Activities

**Audience:** Beginners learning Linux fundamentals for a Cloud Engineer (CE) role  
**Purpose:** Practise the topics in this repository independently.  
**Format:** Two activities for each main chapter.

## Instructions

- Complete the activities in your own Linux lab and record what you observe.
- Activities describe requirements and scenarios only. They intentionally do **not** provide commands, command names, hints, or solutions.
- For each activity, write down the steps you took, the result you expected, the result you observed, and any issue you encountered.
- Use a disposable lab environment for tasks that could change system configuration or affect data.
- Do not partition, format, or modify a real system disk for practice. Disk-management activities involving destructive operations must use a disposable virtual machine and a separate test disk.
- Some service, boot, hardware, partition, and system-management tasks may not work in a basic Docker container. Use a suitable Linux VM for those tasks.
- Do not store passwords, private keys, tokens, or other secrets in this repository.

---

# Chapter 01 — Getting Started

*Topics include Linux fundamentals, architecture, distributions, environment setup, and package management.*

## Activity 1.1 — Explore Your Linux Environment

### Scenario
You have been given access to a Linux environment and need to understand what kind of system you are working on before performing any administration.

### Requirements
1. Identify the Linux distribution and its version.
2. Identify the running kernel version and processor architecture.
3. Determine whether you are working inside a container, a virtual machine, or another environment, using the information available to you.
4. Explore the available system resources and record the number of processors and the amount of memory visible to the environment.
5. Write a short explanation of the roles of the kernel, shell, system utilities, and user applications.
6. Explain two practical differences between using a Linux container and a full Linux virtual machine.

### Completion criteria
- Record the distribution, version, kernel, architecture, and environment type.
- Provide a short explanation of the major Linux components.
- Describe any information that your environment does not expose.

## Activity 1.2 — Package Management and Software Installation

### Scenario
A team member asks you to install a small, legitimate command-line utility on a disposable Linux lab and verify that it is available.

### Requirements
1. Identify which package-management system is used by your distribution.
2. Inspect the configured software sources and determine whether the package information is current.
3. Find a package that is available from a trusted configured repository.
4. Install the package using the distribution's package-management workflow.
5. Verify that the software is installed and can be invoked.
6. Find out which package owns the installed program, where practical.
7. Remove the package after testing if it is not needed for the lab.
8. Explain the difference between a package, a repository, and a package manager.

### Completion criteria
- Record the package selected and the verification result.
- Explain how you established that the package came from a trusted source.
- Leave the lab in a clean state.

---

# Chapter 02 — Folder Structure

## Activity 2.1 — Linux Directory Investigation

### Scenario
You have joined a team that supports Linux servers. Before troubleshooting anything, you need to understand where different types of files are normally stored.

### Requirements
1. Investigate the purpose of the main Linux directories, including the locations for user home directories, system configuration, variable data and logs, temporary files, installed software, and device or process information.
2. Identify which locations are intended for configuration and which commonly contain changing application or system data.
3. Locate your own home directory and identify the temporary directory available on the system.
4. Find examples of system information exposed through virtual filesystems.
5. Create a directory map in your notes, explaining at least eight important directories in your own words.
6. Explain why a directory such as the one used for temporary files should not be treated as permanent storage.

### Completion criteria
- Submit a directory map with at least eight directories.
- Explain the purpose of each directory in one or two sentences.
- Do not change or delete system files during the investigation.

## Activity 2.2 — Find Where Data Belongs

### Scenario
A new application needs configuration files, logs, temporary working files, executable files, and persistent data. You have to recommend appropriate locations on a Linux server.

### Requirements
1. Choose an appropriate location for each of the five types of data.
2. Explain why each selected location is appropriate.
3. Identify which data should be preserved during an application upgrade and which data may be regenerated.
4. Explain the risks of putting application data in a temporary directory.
5. Describe how you would investigate a report that a log file is growing rapidly without deleting anything.

### Completion criteria
- Produce a table mapping each data type to a proposed location and rationale.
- Clearly distinguish configuration, logs, temporary files, executables, and persistent data.

---

# Chapter 03 — User Management

## Activity 3.1 — Create and Manage a Team's Accounts

### Scenario
A small cloud operations team has three members: Alex, Priya, and Jordan. Alex and Priya need to work on the same project files. Jordan needs a separate account for occasional inspection. Administrative access must be limited.

### Requirements
1. Create the three user accounts with appropriate home directories.
2. Create a group for the shared project team and add the appropriate members.
3. Configure a primary group and supplementary group membership where appropriate.
4. Set initial passwords using the lab's approved process.
5. Inspect the account and group records to verify that the configuration is correct.
6. Verify each user's home directory and login shell.
7. Demonstrate how you would identify whether an account is locked.
8. Explain which team members, if any, should receive administrative privileges and justify your decision.

### Completion criteria
- Provide a table of users, their primary groups, supplementary groups, home directories, and administrative access.
- Verify the result rather than relying only on the account-creation output.
- Use a disposable lab and do not reuse real passwords.

## Activity 3.2 — Investigate an Account Access Problem

### Scenario
A user reports that they cannot access a shared project directory. Another team member has left the project and should no longer have access.

### Requirements
1. Inspect the affected user's account status, group membership, home directory, and login shell.
2. Determine whether the access problem is related to account status or group membership.
3. Correct the lab configuration so the active user has the required group membership.
4. Remove the former team member's access to the project group.
5. Demonstrate how you would lock an account temporarily and later restore access.
6. Explain the difference between deleting a user account and deleting that user's files.
7. Identify the risks of deleting an account before confirming ownership of its files and processes.

### Completion criteria
- Record the initial problem, evidence gathered, changes made, and verification result.
- Do not delete shared data as part of this activity.
- Explain the difference between account status, group membership, and filesystem permissions.

---

# Chapter 04 — File Management

## Activity 4.1 — Build a Project Workspace

### Scenario
You are preparing a workspace for a small application. It needs separate directories for documentation, configuration examples, test data, and archived output.

### Requirements
1. Create a parent workspace with four clearly named subdirectories.
2. Create several empty files and text files in the appropriate locations.
3. Copy a file into a second location while keeping the original intact.
4. Move and rename selected files.
5. Inspect file contents using more than one viewing method.
6. List files, including hidden files, and inspect the directory structure.
7. Calculate the size of the workspace and locate a specific test file by name.
8. Remove only the temporary files you created and verify that the remaining project structure is intact.

### Completion criteria
- Provide a final directory tree and a brief description of the operations completed.
- Confirm that copied files remain in both locations and moved files are no longer in their original locations.
- Do not operate on files outside your practice workspace.

## Activity 4.2 — Recover From a File-Organisation Mistake

### Scenario
A teammate accidentally places files in the wrong directory, creates duplicate copies, and leaves several empty directories behind. You must reorganise the workspace without losing useful data.

### Requirements
1. Create a practice directory containing files with different names and contents, plus nested directories.
2. Identify duplicate or misplaced files based on their names and contents.
3. Reorganise the files into a sensible structure.
4. Preserve one verified copy of each required file.
5. Identify empty directories and remove only those that are no longer needed.
6. Produce an inventory of the final files and directories.
7. Explain the difference between removing an empty directory and removing a directory containing files.

### Completion criteria
- The final workspace has the intended structure.
- All required files remain present and readable.
- Your notes explain how you verified the reorganisation.

---

# Chapter 05 — Vim Shortcuts

## Activity 5.1 — Edit a Configuration-Like Text File

### Scenario
You need to edit a sample configuration file in a terminal-only environment. The file contains several settings, comments, and a few incorrect values.

### Requirements
1. Create a practice text file with at least ten lines, including comments and key-value pairs.
2. Open the file in Vim.
3. Navigate through the file without using a mouse.
4. Add a new setting, modify two existing values, and remove one unnecessary line.
5. Copy one line and place the copy elsewhere in the file.
6. Search for a particular setting and move between matching results.
7. Save the changes and exit the editor.
8. Reopen the file and verify that the changes persisted.

### Completion criteria
- The file contains the intended edits.
- Record which navigation, editing, search, save, and exit actions you practised.
- Do not edit real system configuration files for this activity.

## Activity 5.2 — Correct a Text File Without Losing Changes

### Scenario
You are editing a long sample file and make an accidental change. You need to recover the intended content and finish the edit safely.

### Requirements
1. Create a sample file with multiple sections and repeated words.
2. Make an intentional incorrect edit.
3. Undo the incorrect edit and verify the original text is restored.
4. Search for a repeated word and navigate between occurrences.
5. Select and modify a small section of text.
6. Save the corrected file under a new filename while retaining the original.
7. Open both files and compare their contents.

### Completion criteria
- The original file remains unchanged.
- The new file contains only the intended modifications.
- Your notes describe how you recovered from the mistake.

---

# Chapter 06 — File Permissions

## Activity 6.1 — Shared Project Directory

### Scenario
A development team needs a shared directory. Team members should be able to create and edit project files. Other users should not be able to modify the shared work. New files and directories should follow the team's intended group-sharing policy.

### Requirements
1. Create a practice group and two test users.
2. Create a shared project directory owned by an appropriate administrative account and associated with the project group.
3. Configure directory permissions so the intended users can collaborate while unrelated users have restricted access.
4. Configure group inheritance for newly created items where appropriate.
5. Log in or switch between the test identities and verify which operations succeed or fail.
6. Create files and nested directories as different team members, then inspect their ownership and permissions.
7. Explain the difference between owner, group, and other permissions.
8. Explain why directory execute permission has a different practical effect from file execute permission.

### Completion criteria
- Provide a permission and ownership table for the directory and test files.
- Demonstrate that authorised users can collaborate and unauthorised users are restricted.
- Perform all changes only in the disposable lab.

## Activity 6.2 — Diagnose and Fix a Permission Problem

### Scenario
An application cannot read a configuration file, a shared directory has incorrect ownership, and users should not be able to delete one another's files in a shared temporary workspace.

### Requirements
1. Create a practice setup that reproduces each of the three permission problems.
2. Inspect the ownership and permissions of the affected files and directories.
3. Correct the configuration file's access so the intended application user can read it without granting unnecessary access to everyone.
4. Correct the shared directory's ownership and group access.
5. Configure the shared temporary workspace so users can create their own files but cannot delete other users' files solely because the directory is shared.
6. Explain the purpose and risks of special permission bits, including setuid, setgid, and the sticky bit.
7. Explain the difference between changing an owner and changing a group.
8. Verify the result using the test users.

### Completion criteria
- Each initial problem is reproduced, diagnosed, corrected, and retested.
- Document the minimum permissions needed for each scenario.
- Do not apply broad permissions to the entire system to make the errors disappear.

---

# Chapter 07 — Process Management

## Activity 7.1 — Inspect and Manage Processes

### Scenario
A Linux server appears slow. You need to inspect running processes and safely manage a test process without affecting unrelated services.

### Requirements
1. Inspect running processes and identify their process IDs, owners, and resource usage.
2. Start a harmless long-running test process in your lab.
3. Locate that process using more than one inspection method.
4. Observe its resource usage over time.
5. Send it a normal termination request and verify that it has exited.
6. Start the test process again and practise stopping it using a stronger signal only after the normal termination method is unsuccessful.
7. Explain the difference between a process ID and a parent process ID.
8. Explain why killing a process should not be the first response to every performance issue.

### Completion criteria
- Record the test process, how you identified it, and how you verified termination.
- Do not terminate system processes or processes belonging to other users.

## Activity 7.2 — Investigate a Stuck Application

### Scenario
A test application appears unresponsive. A second test task is running in the background, and you need to understand how process state and priority affect the system.

### Requirements
1. Run two harmless test tasks with different durations.
2. Inspect their process states and parent-child relationships where visible.
3. Place a suitable foreground task in the background and bring it back to the foreground.
4. Inspect the process priority and nice value of a test process.
5. Change the priority of a harmless test process where your permissions allow it.
6. Compare the process information before and after the change.
7. Explain the difference between stopping a job, terminating a process, and restarting a service.
8. Write a short investigation report describing how you would handle a genuinely stuck production application.

### Completion criteria
- Record process IDs, observed states, and the outcome of each operation.
- Use only harmless test tasks and do not change priorities of production processes.

---

# Chapter 08 — System Monitoring

## Activity 8.1 — Create a Linux Resource Snapshot

### Scenario
You have been asked to provide a basic health snapshot of a Linux server before a deployment.

### Requirements
1. Record CPU and load information available in the environment.
2. Record total, used, available, and swap memory information.
3. Record filesystem capacity and available space.
4. Identify the processes consuming the most CPU and memory at the time of inspection.
5. Inspect basic network-interface information and current network statistics where available.
6. Identify where system logs can be inspected for recent warnings or errors.
7. Present the findings in a table with the metric, observed value, and a short interpretation.
8. Explain why one snapshot is not enough to establish a long-term performance trend.

### Completion criteria
- Submit a concise resource report.
- Clearly distinguish observed facts from assumptions.
- Note any metrics unavailable because of the lab environment.

## Activity 8.2 — Diagnose a Resource Alert

### Scenario
A monitoring alert reports that disk usage is high and the application seems slower than usual. You need to investigate without immediately deleting data or restarting services.

### Requirements
1. Inspect current CPU, memory, swap, and filesystem usage.
2. Identify whether a filesystem is nearly full and investigate which directories account for significant usage.
3. Inspect running processes and look for unusual resource consumption.
4. Review recent relevant logs for errors or warnings.
5. Determine whether the available evidence supports a disk, memory, CPU, or application-level problem.
6. Propose the next safe investigation step and explain why it is appropriate.
7. Write a short incident report with symptoms, evidence, likely causes, and recommended next actions.
8. Explain why deleting large files or restarting services without understanding the cause can make an incident worse.

### Completion criteria
- Submit a structured troubleshooting report.
- Do not delete files or stop services unless the lab explicitly authorises that action.
- Distinguish confirmed findings from possible causes.

---

# Chapter 09 — Disk and Storage Management

> **Safety requirement:** Use a disposable virtual machine and a separate test disk for activities involving partitioning, filesystem creation, LVM, or swap. Never format or repartition your host's disk or the active system disk. A typical Docker container may not expose suitable block devices; use an appropriate VM for the advanced tasks.

## Activity 9.1 — Inspect Storage and Mount a Test Filesystem

### Scenario
A Linux server has an additional empty test disk. You must identify the device and make its storage available at a designated directory.

### Requirements
1. Inspect available block devices, partitions, filesystem types, UUIDs, and mount points.
2. Identify the separate test disk and verify that it is not the system disk.
3. Record the disk's size and current partition layout.
4. In the disposable lab only, create a partition on the test disk and create an appropriate filesystem on that partition.
5. Create a mount-point directory and mount the new filesystem there.
6. Verify that the filesystem is mounted and available.
7. Create a test file on the mounted filesystem and verify that it is stored through that mount.
8. Unmount the filesystem after confirming that it is safe to do so.
9. Explain the difference between a disk, partition, filesystem, and mount point.

### Completion criteria
- Provide before-and-after storage inventories.
- Identify the test device, partition, filesystem, UUID, and mount point.
- Confirm that no operation was performed on the host or active system disk.

## Activity 9.2 — Persistent Storage, Capacity, and Recovery Planning

### Scenario
An application requires a separate data volume that should be available after a reboot. The operations team also wants a basic capacity report and a recovery plan.

### Requirements
1. Use a disposable test filesystem and record its UUID and filesystem type.
2. Configure the test filesystem for persistent mounting using the system's filesystem table.
3. Validate the configuration before rebooting and verify that the filesystem is mounted at the expected location afterward.
4. Inspect filesystem capacity and directory usage, and record the results.
5. Inspect available swap information and explain the role of swap in Linux.
6. If your lab supports LVM, identify the roles of a physical volume, volume group, and logical volume. Do not perform LVM changes on production or system storage.
7. Write a recovery checklist for a mount that fails after reboot, including how you would check configuration, identifiers, logs, and mount status.
8. Explain the difference between making a filesystem available now and configuring it to mount automatically.
9. Document how you would safely detach or clean up the disposable test storage.

### Completion criteria
- Provide a storage configuration record and a short troubleshooting checklist.
- Verify the persistent mount rather than assuming the configuration worked.
- Leave the lab in a documented, safe state.
- If the environment cannot support reboot, LVM, or swap exercises, document the limitation and complete the conceptual investigation in preparation for an AWS VM lab.

---

# Final Self-Assessment

After completing the activities, check whether you can do each of the following without copying a walkthrough:

- [ ] Explain the Linux architecture and identify the distribution and environment.
- [ ] Explain the purpose of major Linux directories.
- [ ] Create users and groups and investigate account-access problems.
- [ ] Organise files and directories without losing data.
- [ ] Edit text files in Vim and recover from an accidental edit.
- [ ] Configure and troubleshoot file and directory permissions.
- [ ] Inspect, prioritise, and safely terminate a test process.
- [ ] Collect a basic system-health snapshot and investigate a resource alert.
- [ ] Explain disk, partition, filesystem, mount point, UUID, and persistent mounting.
- [ ] Identify which tasks need a full Linux VM rather than a Docker container.

## Suggested Lab Report Template

Use this template for each activity:

- **Activity:**
- **Objective:**
- **Environment:**
- **Initial state:**
- **Work completed:**
- **Expected result:**
- **Observed result:**
- **Problems encountered:**
- **How I verified the result:**
- **Cleanup completed:**
- **What I learned:**

## Important Learning Boundary

This repository focuses on Linux fundamentals. Networking and shell scripting are separate learning phases in the roadmap. Basic command-line inspection and filtering are included here because they help with Linux troubleshooting; writing scripts and automating workflows can be practised later.
