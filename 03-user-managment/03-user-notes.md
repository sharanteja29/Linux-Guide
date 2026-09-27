# Linux User Management

## 1. Introduction

Linux is a **multi-user operating system**, which means multiple users can use the same Linux system.

User management is the process of creating, modifying, deleting, and controlling user accounts and their access to system resources.

Linux uses **users, groups, permissions, and privileges** to control access.

### Core Concept

```text
User
  ↓
Belongs to one or more Groups
  ↓
Permissions determine access
  ↓
Sudo provides administrative privileges
  ↓
SSH provides remote access
```

---

## 2. What is a User?

A **user** is an account that represents a person, service, application, or process on a Linux system.

Examples:

```text
root
sharan
john
ubuntu
developer
```

Every user has a unique **UID (User ID)**.

### Check Current User

```bash
whoami
```

Example:

```text
sharan
```

### View User Identity

```bash
id
```

Example:

```text
uid=1000(sharan) gid=1000(sharan) groups=1000(sharan),27(sudo)
```

Here:

- `uid=1000` → User ID
- `sharan` → Username
- `gid=1000` → Primary Group ID
- `sudo` → Secondary group

---

## 3. Why Do We Need Users?

Users provide **security, isolation, and access control**.

For example, a Linux server may have:

```text
Alice
Bob
Charlie
```

Linux can give each user different access:

```text
Alice   → Project A
Bob     → Project B
Charlie → Project C
```

Users allow Linux to:

- Identify who is accessing the system
- Protect personal files
- Control access to resources
- Assign different privileges
- Provide individual SSH accounts
- Track actions performed by different accounts
- Restrict administrative operations

---

## 4. Root User

`root` is the Linux **superuser**.

The root user has very high privileges and can:

- Create users
- Delete users
- Install software
- Modify system configuration
- Access protected files
- Start and stop services
- Change file ownership
- Change permissions

Check the current user:

```bash
whoami
```

If the output is:

```text
root
```

you are operating as the root user.

### Why Shouldn't We Use Root All the Time?

Root can modify almost anything on the system.

A mistake made as root can seriously damage the system.

Therefore, Linux commonly uses a normal user account with `sudo` for administrative tasks.

```text
Normal User
     ↓
    sudo
     ↓
Elevated Privileges
     ↓
Administrative Command
```

---

## 5. What is a Group?

A **group** is a collection of users.

Groups make permission management easier.

For example:

```text
developers
    ├── Alice
    ├── Bob
    └── Charlie
```

Instead of assigning permissions individually:

```text
Alice   → Permission
Bob     → Permission
Charlie → Permission
```

we can assign permissions to the group:

```text
developers → Permission
```

All users belonging to that group can receive the appropriate access.

Groups are especially useful on servers where multiple users need access to the same resources.

---

## 6. Primary and Secondary Groups

A Linux user can belong to multiple groups.

Example:

```text
User: sharan

Primary Group:
    sharan

Secondary Groups:
    sudo
    developers
    docker
```

### Primary Group

Every user has one primary group.

Check it with:

```bash
id username
```

Example:

```bash
id sharan
```

The primary group is shown as the `gid`.

### Secondary Groups

A user can also belong to additional groups.

Example:

```bash
sudo usermod -aG developers username
```

Here:

```text
-a → Append
-G → Supplementary/secondary groups
```

The `-a` is important because without it, existing supplementary group memberships can be replaced.

---

## 7. Important Linux User Management Files

Linux stores local user and group information mainly under `/etc`.

Important files:

```text
/etc/passwd
/etc/shadow
/etc/group
/etc/gshadow
```

These files have different purposes:

```text
/etc/passwd
     ↓
User account information

/etc/shadow
     ↓
Password hashes + password aging

/etc/group
     ↓
Group information

/etc/gshadow
     ↓
Secure group information
```

---

## 8. `/etc/passwd`

The `/etc/passwd` file contains information about Linux user accounts.

View it with:

```bash
cat /etc/passwd
```

A typical entry looks like:

```text
sharan:x:1000:1000:Sharan Teja:/home/sharan:/bin/bash
```

The fields are separated by `:`.

### `/etc/passwd` Structure

```text
username:password:UID:GID:GECOS:home_directory:shell
```

Example:

```text
sharan:x:1000:1000:Sharan Teja:/home/sharan:/bin/bash
```

### Fields

| Field | Meaning |
|---|---|
| `sharan` | Username |
| `x` | Password information is stored in `/etc/shadow` |
| `1000` | UID |
| `1000` | Primary GID |
| `Sharan Teja` | GECOS/comment field |
| `/home/sharan` | Home directory |
| `/bin/bash` | Login shell |

> **Important:** `/etc/passwd` does not normally contain the user's password. The `x` indicates that password information is stored in `/etc/shadow`.

---

## 9. `/etc/shadow`

The `/etc/shadow` file stores **password hashes** and password-aging information.

View it with:

```bash
sudo cat /etc/shadow
```

A simplified structure is:

```text
username:password_hash:last_change:min:max:warn:inactive:expire:reserved
```

Important information includes:

- Username
- Password hash
- Last password change
- Minimum password age
- Maximum password age
- Password warning period
- Account expiration

Because `/etc/shadow` contains sensitive authentication information, access is restricted.

### `/etc/passwd` vs `/etc/shadow`

```text
/etc/passwd
      ↓
General user account information

/etc/shadow
      ↓
Sensitive password information
```

---

## 10. `/etc/group`

The `/etc/group` file contains group information.

View it with:

```bash
cat /etc/group
```

Example:

```text
developers:x:1001:sharan,john
```

### `/etc/group` Structure

```text
group_name:password:GID:members
```

Example:

```text
developers:x:1001:sharan,john
```

means:

```text
developers
    ↓
Group name

x
    ↓
Password placeholder

1001
    ↓
GID

sharan,john
    ↓
Group members
```

---

## 11. `/etc/gshadow`

`/etc/gshadow` contains secure group information.

View it with:

```bash
sudo cat /etc/gshadow
```

It is related to groups in a similar way that `/etc/shadow` is related to users.

For normal administration, you usually work with:

```text
/etc/group
```

rather than manually modifying `/etc/gshadow`.

---

## 12. Creating Users with `useradd`

The `useradd` command creates a user account.

### Basic Syntax

```bash
sudo useradd username
```

Example:

```bash
sudo useradd john
```

Depending on the distribution and options used, this may not create a home directory.

### Create User with Home Directory

```bash
sudo useradd -m john
```

The `-m` option creates the user's home directory.

Example:

```text
/home/john
```

---

## 13. Creating a User with a Specific Shell

You can specify the user's login shell:

```bash
sudo useradd -m -s /bin/bash john
```

Here:

```text
-m
↓
Create home directory

-s /bin/bash
↓
Set Bash as login shell
```

Check the user's information:

```bash
grep john /etc/passwd
```

---

## 14. Setting a User Password

After creating a user, set their password:

```bash
sudo passwd john
```

Linux will prompt you to enter and confirm the password.

To change your own password:

```bash
passwd
```

---

## 15. `useradd` vs `adduser`

Both commands can create users, but they work differently.

### `useradd`

```bash
sudo useradd -m john
```

`useradd` is a lower-level utility.

It is useful for:

- Automation
- Scripts
- Explicit configuration
- System administration

### `adduser`

On Debian/Ubuntu:

```bash
sudo adduser john
```

`adduser` is more interactive and user-friendly.

It may ask for:

```text
Password
Full name
Room number
Phone number
```

### Comparison

| `useradd` | `adduser` |
|---|---|
| Lower-level utility | User-friendly wrapper |
| Good for automation | Interactive |
| More explicit options | Easier for beginners |
| Common across Linux | Common on Debian/Ubuntu |

---

## 16. Viewing Users

View local users through `/etc/passwd`:

```bash
cat /etc/passwd
```

Search for a specific user:

```bash
grep john /etc/passwd
```

You can also use:

```bash
getent passwd
```

`getent` can retrieve users from configured identity sources, not only `/etc/passwd`.

---

## 17. Viewing User Information

Use:

```bash
id username
```

Example:

```bash
id john
```

Possible output:

```text
uid=1001(john) gid=1001(john) groups=1001(john),1002(developers)
```

View group membership:

```bash
groups john
```

---

## 18. Modifying Users with `usermod`

`usermod` modifies existing user accounts.

### Change Username

```bash
sudo usermod -l newname oldname
```

Example:

```bash
sudo usermod -l johnny john
```

### Change Home Directory

```bash
sudo usermod -d /new/home/directory -m username
```

Example:

```bash
sudo usermod -d /home/johnny -m johnny
```

The `-m` option moves the existing home directory contents.

### Change Login Shell

```bash
sudo usermod -s /bin/zsh username
```

Example:

```bash
sudo usermod -s /bin/bash john
```

---

## 19. Creating Groups

Create a group:

```bash
sudo groupadd developers
```

Check the group:

```bash
getent group developers
```

---

## 20. Adding Users to Groups

Add a user to a group:

```bash
sudo usermod -aG developers john
```

Verify:

```bash
groups john
```

or:

```bash
id john
```

### Important: `-aG`

Use:

```bash
sudo usermod -aG developers john
```

instead of:

```bash
sudo usermod -G developers john
```

because `-G` without `-a` can replace the user's existing supplementary groups.

---

## 21. Changing the Primary Group

Change the primary group:

```bash
sudo usermod -g developers john
```

Verify:

```bash
id john
```

The primary group appears as the `gid`.

---

## 22. Removing Users

Remove a user:

```bash
sudo userdel john
```

This removes the user account but normally leaves the user's home directory.

To remove the user and their home directory:

```bash
sudo userdel -r john
```

> **Warning:** The `-r` option deletes the user's home directory and its contents.

---

## 23. Locking and Unlocking User Accounts

Temporarily lock a user's password:

```bash
sudo passwd -l username
```

Unlock it:

```bash
sudo passwd -u username
```

This can be useful for temporarily preventing password-based authentication.

---

## 24. Password Expiration with `chage`

Linux can enforce password-aging policies.

Set the maximum password age to 90 days:

```bash
sudo chage -M 90 username
```

View password-aging information:

```bash
sudo chage -l username
```

Useful information includes:

```text
Last password change
Password expires
Password inactive
Account expires
Minimum password age
Maximum password age
Password warning period
```

---

## 25. Sudo and Administrative Privileges

A normal user cannot perform every administrative operation.

For example:

```bash
apt update
```

may require elevated privileges.

With `sudo`:

```bash
sudo apt update
```

The basic concept is:

```text
Normal User
     ↓
    sudo
     ↓
Elevated Privileges
     ↓
Administrative Command
```

`sudo` allows an authorized user to execute commands with elevated privileges.

---

## 26. Adding a User to the Sudo Group

On Debian/Ubuntu:

```bash
sudo usermod -aG sudo username
```

Example:

```bash
sudo usermod -aG sudo john
```

On many RHEL-based distributions:

```bash
sudo usermod -aG wheel username
```

Verify:

```bash
groups john
```

The user may need to log out and log back in before the new group membership is reflected in a new session.

---

## 27. Sudoers Configuration

The sudo configuration is commonly stored in:

```text
/etc/sudoers
```

Do not casually edit this file using a normal text editor.

Use:

```bash
sudo visudo
```

`visudo` checks the syntax before saving.

Example rule:

```text
username ALL=(ALL) /path/to/command
```

Passwordless example:

```text
username ALL=(ALL) NOPASSWD: /path/to/command
```

Use `NOPASSWD` carefully because it allows the specified command without a password prompt.

---

# 28. SSH and Linux Users

SSH allows users to remotely connect to a Linux server.

Basic flow:

```text
Client
   │
   │ SSH Request
   ↓
Linux Server
   │
   ├── Authentication
   ├── User Identification
   └── Authorization
           ↓
       User Session
```

Example:

```bash
ssh john@192.168.1.100
```

Here:

```text
ssh
 ↓
SSH client

john
 ↓
Linux username

192.168.1.100
 ↓
Server IP address
```

The request essentially means:

```text
"Connect to the SSH service
on this server as user john."
```

---

## 29. What Happens During an SSH Login?

A simplified SSH login process:

```text
1. Client connects to server
          ↓
2. SSH server responds
          ↓
3. Secure connection is negotiated
          ↓
4. Client specifies the user
          ↓
5. Server authenticates the user
          ↓
6. Server checks whether login is allowed
          ↓
7. User session is created
          ↓
8. User receives a shell
```

For example:

```bash
ssh john@server-ip
```

The server needs to determine:

```text
Does john exist?
        ↓
Is SSH login allowed?
        ↓
Can john authenticate?
        ↓
What is john's shell?
        ↓
What permissions does john have?
        ↓
Create john's session
```

---

## 30. SSH Authentication Methods

SSH commonly uses two authentication methods.

### Password Authentication

```bash
ssh john@server-ip
```

The server asks for John's password.

### SSH Key Authentication

SSH can use a public/private key pair:

```text
Client                         Server
  │                              │
  │ Private Key                  │
  │                              │
  │──── Authentication ─────────→│
  │                              │
  │                         Public Key
  │                      authorized_keys
```

The private key should remain on the client.

The public key is commonly stored on the server in:

```text
~/.ssh/authorized_keys
```

The server uses the public key to verify that the connecting client possesses the corresponding private key.

---

# 31. User Management Big Picture

```text
                    Linux System
                         │
             ┌───────────┴───────────┐
             ↓                       ↓
           Users                   Groups
             │                       │
             └───────────┬───────────┘
                         ↓
                    Permissions
                         │
             ┌───────────┴───────────┐
             ↓                       ↓
        File Access             Commands
             │                       │
             └───────────┬───────────┘
                         ↓
                  Sudo / Privileges
                         │
                         ↓
                  SSH Remote Access
```

### Key Idea

> **Users identify who you are. Groups organize users. Permissions control what they can access. Sudo provides controlled administrative privileges. SSH provides remote access to user accounts.**

---

# 32. Important Commands Cheat Sheet

| Task | Command |
|---|---|
| Show current user | `whoami` |
| Show user ID/groups | `id` |
| Show user information | `id username` |
| Show user's groups | `groups username` |
| Create user | `sudo useradd username` |
| Create user + home | `sudo useradd -m username` |
| Interactive user creation | `sudo adduser username` |
| Set password | `sudo passwd username` |
| Modify user | `sudo usermod ...` |
| Delete user | `sudo userdel username` |
| Delete user + home | `sudo userdel -r username` |
| Create group | `sudo groupadd groupname` |
| Add user to group | `sudo usermod -aG group username` |
| Change primary group | `sudo usermod -g group username` |
| Lock account | `sudo passwd -l username` |
| Unlock account | `sudo passwd -u username` |
| Password aging | `sudo chage -l username` |
| Add to sudo group | `sudo usermod -aG sudo username` |
| Edit sudo configuration | `sudo visudo` |
| Connect through SSH | `ssh username@server-ip` |

---

# 33. Important Files Cheat Sheet

| File | Purpose |
|---|---|
| `/etc/passwd` | User account information |
| `/etc/shadow` | Password hashes and password-aging information |
| `/etc/group` | Group information |
| `/etc/gshadow` | Secure group information |
| `/etc/sudoers` | Sudo authorization rules |
| `~/.ssh/authorized_keys` | Authorized SSH public keys for a user |

---

# 34. Learning Flow

```text
Users
  ↓
UID / GID
  ↓
Groups
  ↓
/etc/passwd
  ↓
/etc/shadow
  ↓
useradd / adduser
  ↓
passwd
  ↓
usermod
  ↓
groupadd
  ↓
Permissions
  ↓
sudo
  ↓
SSH
  ↓
SSH Keys
```

---

# 35. Next Topic: Linux File Permissions

User management leads directly into **Linux File Permissions**.

```text
Users + Groups
      ↓
File Ownership
      ↓
Owner / Group / Others
      ↓
rwx Permissions
      ↓
chmod
      ↓
chown
      ↓
chgrp
      ↓
umask
```

The key relationship is:

```text
User
  ↓
Group
  ↓
File Ownership
  ↓
Permissions
  ↓
Access Control
```