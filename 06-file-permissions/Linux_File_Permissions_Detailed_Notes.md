# Linux File Permissions --- Detailed Notes

## 1. What Are File Permissions?

Linux is a multi-user operating system. Multiple users can work on the
same server, so Linux needs a mechanism to control who can access files
and directories.

File permissions answer: - **Who** can access an object? - **What** can
they do? - **What** are they prevented from doing?

They complement Linux user and group management.

------------------------------------------------------------------------

## 2. Permission Classes

Every file/directory has three permission classes:

  Class   Meaning
  ------- --------------
  `u`     User / owner
  `g`     Group
  `o`     Others

**Owner:** the user associated with the file.

**Group:** the file's associated group. Group permissions can apply to
members of that group.

**Others:** users who are not the owner and do not receive access
through the applicable group class.

------------------------------------------------------------------------

## 3. Basic Permissions

  Permission     Symbol   Numeric value General meaning
  ------------ -------- --------------- ---------------------------------------------
  Read              `r`             `4` Read file contents / list directory entries
  Write             `w`             `2` Modify file / modify directory entries
  Execute           `x`             `1` Execute a file / traverse a directory
  None              `-`             `0` Permission absent

The normal order is:

``` text
rwx
```

------------------------------------------------------------------------

## 4. Understanding `ls -l`

Use:

``` bash
ls -l
```

Example:

``` text
-rwxr--r-- 1 user group 1234 Mar 28 10:00 myfile.sh
```

The permission field is:

``` text
-rwxr--r--
```

Breakdown:

``` text
- rwx r-- r--
│ │   │   │
│ │   │   └── Others
│ │   └────── Group
│ └────────── Owner
└──────────── File type
```

The first character identifies the object type. Common values:

``` text
-  regular file
d  directory
```

The following nine characters are three sets of three:

``` text
rwx rwx rwx
│   │   │
│   │   └── Others
│   └────── Group
└────────── Owner
```

------------------------------------------------------------------------

## 5. Reading a Permission String

Example:

``` text
-rwxr-xr--
```

Ignore the first character and split the rest:

``` text
rwx | r-x | r--
```

Therefore:

``` text
Owner  → rwx → read, write, execute
Group  → r-x → read, execute
Others → r-- → read only
```

A `-` means that permission is absent.

For example:

``` text
rw-
```

means read + write, but no execute.

------------------------------------------------------------------------

## 6. Why Permissions Matter

Imagine a server containing developers, QA engineers and DevOps
engineers.

If every user could freely modify or delete every other user's files,
one user could accidentally damage another user's work or important
system data.

A typical example:

-   Developer creates a script.
-   QA needs to read it.
-   QA should not necessarily modify it.
-   Linux permissions can enforce this separation.

The course demonstrates this with a developer and QA user: the QA user
can read another user's file under the appropriate default permissions
but cannot modify/delete it.

------------------------------------------------------------------------

# 7. `chmod` --- Change Permissions

`chmod` means **change mode** and is used to modify permission bits.

There are two major forms:

1.  Symbolic mode
2.  Numeric/octal mode

------------------------------------------------------------------------

## 8. Symbolic `chmod`

The classes are:

``` text
u = user
g = group
o = others
```

Operators:

``` text
+ = add
- = remove
= = set exactly
```

Examples:

``` bash
chmod u+x filename
chmod g-w filename
chmod o=r filename
```

Meaning:

``` text
u+x → add execute for owner
g-w → remove write from group
o=r → set others to read only
```

Multiple changes can be combined:

``` bash
chmod u=rwx,g=rx,o= filename
```

Result:

``` text
rwx r-x ---
```

------------------------------------------------------------------------

## 9. Numeric / Octal `chmod`

The values are:

``` text
r = 4
w = 2
x = 1
```

Add them for each class.

### Examples

``` text
rwx = 4 + 2 + 1 = 7
rw- = 4 + 2     = 6
r-x = 4 + 1     = 5
r-- = 4         = 4
--- = 0
```

Important table:

    Number Permission
  -------- ------------
       `7` `rwx`
       `6` `rw-`
       `5` `r-x`
       `4` `r--`
       `3` `-wx`
       `2` `-w-`
       `1` `--x`
       `0` `---`

The three digits always represent:

``` text
OWNER GROUP OTHERS
```

------------------------------------------------------------------------

## 10. Common Numeric Permissions

### `755`

``` text
7   5   5
rwx r-x r-x
```

Owner: read/write/execute

Group: read/execute

Others: read/execute

### `644`

``` text
6   4   4
rw- r-- r--
```

Owner: read/write

Group: read

Others: read

### `700`

``` text
7   0   0
rwx --- ---
```

Only owner has access.

### `600`

``` text
6   0   0
rw- --- ---
```

Only owner has read/write.

### `400`

``` text
4   0   0
r-- --- ---
```

Only owner can read.

### `444`

``` text
4   4   4
r-- r-- r--
```

Everyone can read; nobody can write or execute.

### `777`

``` text
7   7   7
rwx rwx rwx
```

Everyone has all three basic permissions.

**Security note:** `777` is extremely permissive and should not be used
as a generic fix for `Permission denied`.

------------------------------------------------------------------------

## 11. Fast Conversion

Example:

``` text
rwxr-xr--
```

Split:

``` text
rwx | r-x | r--
```

Convert:

``` text
rwx = 7
r-x = 5
r-- = 4
```

Therefore:

``` text
754
```

------------------------------------------------------------------------

# 12. File vs Directory Permissions

The same `r`, `w`, and `x` bits exist for files and directories, but
their practical meanings differ.

## Regular file

``` text
r → read contents
w → modify contents
x → execute
```

## Directory

``` text
r → list directory entries
w → modify directory entries
x → traverse/search the directory
```

This distinction is extremely important.

For a directory, **execute does not mean "run the directory."** It means
the ability to traverse/search it as part of accessing paths.

------------------------------------------------------------------------

# 13. Directory Permission Example

Think:

``` text
project/
├── app.py
├── config/
└── logs/
```

Directory permissions control access to the directory itself and the
ability to traverse it.

File permissions on `app.py` separately control what can be done to that
file.

Therefore both levels can matter:

``` text
Directory/path permissions
        +
File permissions
        =
Actual access
```

------------------------------------------------------------------------

# 14. Parent Directory / Path Traversal

The course uses a **bank and locker** analogy:

``` text
Bank
└── Locker
```

The bank represents the directory and the locker represents the file.

You must first be able to get through the bank before you can reach the
locker.

Similarly:

``` text
/tmp
└── demo
```

Even if `demo` has very open permissions, a user cannot access it
through `/tmp` if they cannot traverse `/tmp`.

### Important clarification

It is more precise to think of directory permissions as a **prerequisite
for path traversal**, rather than saying directory permissions always
"take higher priority" than file permissions.

For a path such as:

``` text
/var/app/config/settings.conf
```

the relevant traversal permissions on the path must be satisfied, and
then the permissions on the final object determine the requested
operation.

------------------------------------------------------------------------

# 15. `chown` --- Change Ownership

`chown` changes ownership.

Basic form:

``` bash
chown newuser filename
```

Change owner and group:

``` bash
chown newuser:newgroup filename
```

Change only group:

``` bash
chown :newgroup filename
```

Recursively:

``` bash
chown -R newuser:newgroup directory/
```

The course demonstrates that changing ownership generally requires
appropriate administrative privileges.

------------------------------------------------------------------------

# 16. `chgrp` --- Change Group Ownership

The additional material also covers:

``` bash
chgrp newgroup filename
```

For a directory and its contents:

``` bash
chgrp -R newgroup directory/
```

Mental model:

``` text
chmod → permissions
chown → owner/group ownership
chgrp → group ownership
```

------------------------------------------------------------------------

# 17. Ownership vs Permissions

Consider:

``` text
-rw-r----- 1 developer developers 1234 test.sh
```

There are two separate concepts:

``` text
developer  → owner
developers → group
rw-r-----  → permissions
```

Changing ownership does not mean you changed the permission bits.

Changing permissions does not mean you changed the owner.

------------------------------------------------------------------------

# 18. Special Permissions

The additional material you provided goes beyond the original course
transcript and introduces:

-   SetUID
-   SetGID
-   Sticky Bit

These are important Linux concepts.

------------------------------------------------------------------------

## 18.1 SetUID

SetUID is represented by `s` in the owner execute position.

Conceptually, a SetUID executable can run with the effective
identity/privileges associated with its owner.

Enable:

``` bash
chmod u+s filename
```

The provided material uses `/usr/bin/passwd` as an example.

Because SetUID can affect privilege boundaries, it should be treated
carefully.

------------------------------------------------------------------------

## 18.2 SetGID

SetGID is represented by `s` in the group execute position.

For executable files, it can provide the file's group identity during
execution.

For directories, an important behavior is group inheritance:

``` text
shared directory
      ↓
new files/subdirectories
      ↓
inherit the directory's group
```

Enable:

``` bash
chmod g+s directory/
```

This is useful for shared team directories.

------------------------------------------------------------------------

## 18.3 Sticky Bit

Sticky Bit is represented by `t` in the others execute position.

It is commonly seen on shared writable directories such as:

``` text
/tmp
```

The important behavior is that users cannot freely delete or rename
other users' entries merely because the shared directory is writable;
ownership and privilege rules still apply.

Enable:

``` bash
chmod +t directory/
```

------------------------------------------------------------------------

## 18.4 Special Permission Summary

  -----------------------------------------------------------------------
  Permission              Symbol                  Main idea
  ----------------------- ----------------------- -----------------------
  SetUID                  `s`                     Executable can use
                                                  owner's effective
                                                  identity

  SetGID                  `s`                     Group identity for
                                                  execution; directory
                                                  group inheritance

  Sticky Bit              `t`                     Restricts
                                                  deletion/rename of
                                                  other users' entries in
                                                  shared writable
                                                  directories
  -----------------------------------------------------------------------

------------------------------------------------------------------------

# 19. `umask`

The additional material also covers:

``` bash
umask
```

`umask` influences the default permission bits used when new files and
directories are created.

Check it:

``` bash
umask
```

Example:

``` bash
umask 022
```

A commonly taught result is:

``` text
Directories → 755
Regular files → 644
```

The important concept is:

``` text
umask → influences permissions of newly created objects
```

It is a creation-time mask, not simply a command that blindly runs
`chmod 755` or `chmod 644`.

------------------------------------------------------------------------

# 20. Permission Troubleshooting

When you see:

``` text
Permission denied
```

use a structured process.

### Step 1 --- Identify the object

Is it:

``` text
file?
directory?
script?
configuration?
```

### Step 2 --- Inspect permissions

Read the `rwx` bits.

### Step 3 --- Check owner

Who owns it?

### Step 4 --- Check group

Which group owns it?

### Step 5 --- Identify the accessing user

Who is attempting the operation?

### Step 6 --- Determine the applicable class

Is that user:

``` text
owner?
group member?
other?
```

### Step 7 --- Check the path

Can the user traverse all required parent directories?

### Step 8 --- Grant only what is required

Do not immediately use `777`.

------------------------------------------------------------------------

# 21. Least Privilege

A strong Linux security principle is:

> Give users only the access they actually need.

If a user only needs to read:

``` text
give read
```

If a user needs to execute:

``` text
give execute
```

If a user needs to modify:

``` text
give write
```

Do not grant unnecessary access.

Bad troubleshooting habit:

``` bash
chmod 777 file
```

Better approach:

``` text
Understand the required operation
        ↓
Identify the correct user/group
        ↓
Grant the minimum required permission
```

------------------------------------------------------------------------

# 22. Common Mistakes

### Mistake 1: Confusing `chmod` and `chown`

``` text
chmod → permissions
chown → ownership
```

### Mistake 2: Thinking `777` is a universal solution

It is not.

### Mistake 3: Looking only at the final file

Parent-directory traversal can prevent access.

### Mistake 4: Forgetting the owner/group fields

Always inspect:

``` text
permissions + owner + group
```

### Mistake 5: Forgetting that directory `x` means traversal

For directories:

``` text
x ≠ "run the directory"
x = traverse/search
```

### Mistake 6: Reading all nine characters as one value

Always split:

``` text
rwx | rwx | rwx
```

------------------------------------------------------------------------

# 23. Practical Permission Troubleshooting Example

Suppose:

``` text
/var/app/config/settings.conf
```

produces:

``` text
Permission denied
```

Do not immediately modify `settings.conf`.

Think:

``` text
Who am I?
      ↓
What is the file owner?
      ↓
What is the file group?
      ↓
What are the file permissions?
      ↓
Am I owner/group/other?
      ↓
Can I traverse /var?
      ↓
Can I traverse /var/app?
      ↓
Can I traverse /var/app/config?
      ↓
Do I have the required permission on settings.conf?
```

This mindset is much more useful for Cloud/DevOps troubleshooting than
memorizing random `chmod` commands.

------------------------------------------------------------------------

# 24. Cloud Engineer / DevOps Relevance

Linux permissions appear constantly in:

-   application deployment
-   configuration management
-   shell scripts
-   CI/CD pipelines
-   web servers
-   logs
-   SSH
-   certificates and private keys
-   Docker volume mounts
-   service accounts
-   system services
-   shared project directories

Typical failures include:

``` text
script cannot execute
application cannot read configuration
service cannot write logs
deployment cannot modify a directory
user cannot access a mounted volume
SSH key rejected because permissions are too broad
```

Understanding permissions makes these failures much easier to
troubleshoot.

------------------------------------------------------------------------

# 25. CE Priority

## Must know

``` text
ls -l
owner / group / others
r / w / x
chmod
symbolic chmod
numeric chmod
4 / 2 / 1
644
755
700
600
777 concept
chown
file vs directory permissions
parent-directory traversal
```

## Should know

``` text
chgrp
umask
SetUID
SetGID
Sticky Bit
```

------------------------------------------------------------------------

# 26. Core Cheat Sheet

``` text
u = user/owner
g = group
o = others

r = read  = 4
w = write = 2
x = execute/traverse = 1

chmod = change permissions
chown = change ownership
chgrp = change group
umask = creation-time permission mask
```

Common modes:

``` text
777 = rwxrwxrwx
755 = rwxr-xr-x
750 = rwxr-x---
700 = rwx------
644 = rw-r--r--
640 = rw-r-----
600 = rw-------
444 = r--r--r--
400 = r--------
```

Fast conversion:

``` text
rwx = 7
rw- = 6
r-x = 5
r-- = 4
--- = 0
```

------------------------------------------------------------------------

# 27. Final Mental Model

When you see:

``` text
-rwxr-x---
```

think:

``` text
Owner  → rwx
Group  → r-x
Others → ---
```

When you see:

``` text
754
```

think:

``` text
7 = rwx
5 = r-x
4 = r--
```

When you see:

``` text
chmod
```

think:

``` text
CHANGE ACCESS
```

When you see:

``` text
chown
```

think:

``` text
CHANGE OWNER
```

When you see:

``` text
Permission denied
```

think:

``` text
USER
  ↓
OWNER / GROUP / OTHERS
  ↓
PERMISSIONS
  ↓
PARENT-DIRECTORY TRAVERSAL
  ↓
REQUIRED OPERATION
```

## Golden Rule

**Do not blindly fix permission problems with `chmod 777`.**

First understand:

``` text
WHO needs access?
WHAT operation is required?
WHICH user/group should receive it?
WHICH directory/path permissions are required?
WHAT is the minimum permission needed?
```

That is the core Linux File Permissions skill you need for Cloud
Engineer/DevOps work.
