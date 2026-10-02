# Linux File Management — Complete Notes

## 1. Introduction

File management is one of the most important Linux fundamentals for a Cloud/DevOps Engineer.

You will regularly work with:

- Files
- Directories
- Configuration files
- Log files
- Shell scripts
- YAML files
- Application files
- Backup files

Most server administration is performed from the terminal, so you should be comfortable creating, viewing, editing, copying, moving, and deleting files.

---

# 2. Linux Filesystem Basics

Linux has a single filesystem hierarchy that starts from:

```text
/
```

This is called the **root directory**.

It is not the same as the `root` user's home directory.

### Important distinction

```text
/       → Root directory of the filesystem
/root   → Home directory of the root user
/home   → Directory containing normal users' home directories
/home/john → John's home directory
```

For example:

```bash
cd /
```

moves to the filesystem root.

```bash
cd /root
```

moves to the root user's home directory.

```bash
cd /home
```

moves to the directory containing normal users' home directories.

---

# 3. `ls` — List Files and Directories

`ls` displays files and directories in the current location.

```bash
ls
```

Example:

```text
Documents  Downloads  file.txt  scripts
```

## Useful options

### Long listing

```bash
ls -l
```

Shows detailed information such as:

- Permissions
- Owner
- Group
- File size
- Modification time
- File name

### Show hidden files

```bash
ls -a
```

Hidden files normally begin with `.`.

### Long listing + hidden files

```bash
ls -la
```

This is one of the most commonly used forms.

---

# 4. `cd` — Change Directory

Used to move between directories.

```bash
cd /path/to/directory
```

Example:

```bash
cd /home
```

## Go to parent directory

```bash
cd ..
```

`..` means the parent directory.

## Go to your home directory

```bash
cd
```

or:

```bash
cd ~
```

`~` represents the current user's home directory.

For root:

```text
~
```

normally means:

```text
/root
```

For a normal user such as John:

```text
~
```

normally means:

```text
/home/john
```

## Go to filesystem root

```bash
cd /
```

---

# 5. `pwd` — Print Working Directory

Shows your current location.

```bash
pwd
```

Example:

```text
/home/john
```

Whenever you are unsure where you are, use:

```bash
pwd
```

---

# 6. `mkdir` — Create Directory

Creates a directory.

```bash
mkdir directory_name
```

Example:

```bash
mkdir projects
```

## Create nested directories

Use `-p`:

```bash
mkdir -p project/config/logs
```

This creates the complete directory structure if the parent directories don't already exist.

---

# 7. `rmdir` — Remove Empty Directory

Used to remove an **empty** directory.

```bash
rmdir directory_name
```

Example:

```bash
rmdir test
```

If the directory contains files, `rmdir` will fail.

This is important:

```text
rmdir → only removes empty directories
```

---

# 8. `rm` — Remove Files

Remove a file:

```bash
rm file.txt
```

Example:

```bash
rm old.log
```

## Interactive deletion

```bash
rm -i file.txt
```

Linux asks for confirmation before deleting.

## Remove a directory and its contents

```bash
rm -r directory
```

`-r` means recursive.

Example:

```bash
rm -r old_project
```

## `rm -rf`

```bash
rm -rf directory
```

- `-r` → recursive
- `-f` → force

Be very careful with this command.

A mistaken `rm -rf` can delete large amounts of data.

---

# 9. `cp` — Copy

Copy a file:

```bash
cp source.txt destination.txt
```

Example:

```bash
cp app.conf app.conf.backup
```

This creates a copy of `app.conf`.

## Copy a directory

Use `-r`:

```bash
cp -r source_directory destination_directory
```

Example:

```bash
cp -r project project_backup
```

---

# 10. `mv` — Move or Rename

`mv` is used for both moving and renaming.

## Rename a file

```bash
mv old.txt new.txt
```

Example:

```bash
mv app.conf app.conf.backup
```

## Move a file

```bash
mv app.conf backup/
```

The file is moved into the `backup` directory.

## Move and rename

```bash
mv app.conf backup/app.conf.old
```

---

# 11. `cat` — Display File Contents

Displays the contents of a file.

```bash
cat file.txt
```

Example:

```bash
cat app.conf
```

Useful for small files.

For very large files, use `less` instead.

---

# 12. `tac` — Display in Reverse Line Order

```bash
tac file.txt
```

`cat` displays lines from top to bottom.

`tac` displays lines from bottom to top.

---

# 13. `less` — View Large Files

```bash
less file.txt
```

Useful for large configuration files and logs.

Common navigation:

```text
Space       → Next page
b           → Previous page
Up/Down     → Move
/word       → Search
q           → Quit
```

For Cloud/DevOps work, `less` is especially useful for logs.

Example:

```bash
less /var/log/syslog
```

---

# 14. `more` — View File Page by Page

```bash
more file.txt
```

It displays a file one page at a time.

`less` is generally more flexible and commonly preferred.

---

# 15. `head` — View Beginning of File

```bash
head file.txt
```

By default, it displays the first 10 lines.

Specify the number of lines:

```bash
head -n 20 file.txt
```

This displays the first 20 lines.

---

# 16. `tail` — View End of File

```bash
tail file.txt
```

By default, it displays the last 10 lines.

Specify the number:

```bash
tail -n 20 file.txt
```

Displays the last 20 lines.

## `tail -f`

```bash
tail -f app.log
```

This follows the file as new lines are added.

This is extremely useful for monitoring logs.

For example, while an application is running:

```bash
tail -f application.log
```

You can watch new log entries appear.

Press:

```text
Ctrl+C
```

to stop following the file.

---

# 17. `nano` — Simple Terminal Editor

`nano` is an easy terminal text editor.

```bash
nano file.txt
```

You can type and edit normally.

It is generally easier for beginners than `vi`.

For Cloud Engineer work, it is useful to know, but `vi`/`vim` is worth understanding because it is commonly available on Linux servers.

---

# 18. `vi` Editor

`vi` is a terminal-based text editor.

Common uses:

- Configuration files
- Shell scripts
- YAML files
- Server configuration
- Application configuration

Example:

```bash
vi app.conf
```

---

## 18.1 Modes in `vi`

The important concept is that `vi` has different modes.

### Normal Mode

Used for:

- Navigation
- Searching
- Deleting lines
- Copying/pasting
- Undo
- Save/quit commands

When you open a file, you start in Normal Mode.

### Insert Mode

Used for:

- Typing
- Editing
- Normal deletion using `Backspace` or `Delete`

Enter Insert Mode:

```text
i
```

Return to Normal Mode:

```text
Esc
```

### Command-line Mode

Used for:

- Save
- Quit
- Search/replace
- Other editor commands

Enter it using:

```text
:
```

---

# 19. Basic `vi` Editing

## `i`

Enter Insert Mode.

```text
i
```

You can now type and edit the file.

Important:

```text
Normal Mode → i → Insert Mode
Insert Mode → Esc → Normal Mode
```

---

# 20. Deleting in Insert Mode

In Insert Mode you can delete normally:

```text
Backspace
Delete
```

You do not need `x` just to delete a character.

For example:

```text
server=nginx
```

While in Insert Mode, use Backspace/Delete just like a normal editor.

---

# 21. `vi` Navigation

You can use arrow keys for basic navigation.

Traditional `vi` navigation is:

```text
h → left
j → down
k → up
l → right
```

The arrow keys can perform the same basic movement, so `hjkl` does not need to be a priority for your Cloud Engineer basics.

Other useful navigation:

```text
0  → Beginning of line
$  → End of line
gg → Beginning of file
G  → End of file
```

---

# 22. Delete a Line in `vi`

In Normal Mode:

```text
dd
```

deletes the current line.

For example:

```text
server=nginx
port=8080
debug=true
```

Place the cursor on:

```text
port=8080
```

and press:

```text
dd
```

Result:

```text
server=nginx
debug=true
```

---

# 23. Copy and Paste

Copy the current line:

```text
yy
```

Paste:

```text
p
```

These commands are used in Normal Mode.

---

# 24. Undo and Redo

Undo:

```text
u
```

Redo:

```text
Ctrl+r
```

If you accidentally delete a line with:

```text
dd
```

you can use:

```text
u
```

to undo it.

---

# 25. Search in `vi`

Search forward:

```text
/word
```

Example:

```text
/port
```

Press Enter to find the match.

Next match:

```text
n
```

Previous match:

```text
N
```

This is very useful when working with large configuration files.

---

# 26. Save and Exit in `vi`

First press:

```text
Esc
```

### Save

```text
:w
```

### Quit

```text
:q
```

### Save and quit

```text
:wq
```

### Quit without saving

```text
:q!
```

The last one discards your changes.

---

# 27. Search and Replace in `vi`

Replace all occurrences throughout the file:

```text
:%s/old/new/g
```

Example:

```text
:%s/dev/prod/g
```

This replaces `dev` with `prod` throughout the file.

This is useful, but you can learn it after the basic `vi` workflow.

---

# 28. What You Need to Prioritize in `vi`

For your Cloud Engineer preparation:

### Must know

```text
i
Esc

Arrow keys
0
$
gg
G

/word
n
N

dd
yy
p
u

:w
:q
:wq
:q!
```

### Optional / Learn Later

```text
a
A
I
o
O
x
dw
D
:e
:split
:vsplit
```

`a`, `A`, and `I` are mainly convenience shortcuts. You can achieve similar editing by navigating and using `i`.

Similarly, `Backspace` and `Delete` can be used normally while in Insert Mode.

---

# 29. `vi` Practical Workflow

A common workflow is:

```text
vi config.conf
```

Search:

```text
/port
```

Enter editing:

```text
i
```

Modify the configuration.

Return to Normal Mode:

```text
Esc
```

Save and exit:

```text
:wq
```

If you don't want to save:

```text
:q!
```

If you make a mistake:

```text
u
```

---

# 30. `echo` — Display Text

```bash
echo "Hello"
```

Output:

```text
Hello
```

`echo` can also be used to write text into files.

---

# 31. `>` — Redirect and Overwrite

```bash
echo "Hello" > file.txt
```

This creates `file.txt` if it doesn't exist.

If the file already exists, `>` **overwrites its existing contents**.

Example:

```bash
echo "First line" > test.txt
echo "Second line" > test.txt
```

The file will contain only:

```text
Second line
```

---

# 32. `>>` — Redirect and Append

```bash
echo "Hello" >> file.txt
```

This adds the text to the end of the file.

Example:

```bash
echo "First line" > test.txt
echo "Second line" >> test.txt
```

The file contains:

```text
First line
Second line
```

### Important difference

```text
>   → overwrite
>>  → append
```

---

# 33. Important Command Differences

| Command | Purpose |
|---|---|
| `ls` | List files/directories |
| `cd` | Change directory |
| `pwd` | Show current directory |
| `mkdir` | Create directory |
| `rmdir` | Remove empty directory |
| `rm` | Remove file |
| `rm -r` | Remove directory recursively |
| `cp` | Copy |
| `mv` | Move/rename |
| `cat` | Display file |
| `less` | View file page by page |
| `head` | Show beginning |
| `tail` | Show end |
| `tail -f` | Follow changing log |
| `nano` | Simple text editor |
| `vi` | Terminal text editor |
| `echo` | Print/write text |
| `>` | Overwrite file |
| `>>` | Append to file |

---

# 34. Important Differences to Remember

## `rmdir` vs `rm -r`

```text
rmdir directory
```

Only works with an empty directory.

```text
rm -r directory
```

Can remove a directory containing files/subdirectories.

---

## `cp` vs `mv`

```text
cp
```

creates a copy.

```text
mv
```

moves the original.

`mv` is also used for renaming.

---

## `cat` vs `less`

```text
cat file
```

quickly displays the file.

```text
less file
```

is better for large files.

---

## `head` vs `tail`

```text
head file
```

shows the beginning.

```text
tail file
```

shows the end.

```text
tail -f file
```

continuously follows new content.

---

## `>` vs `>>`

```text
>   → overwrite
>>  → append
```

---

# 35. Useful Cloud/DevOps Examples

## Check a configuration file

```bash
cat app.conf
```

## Search through a large file

```bash
less app.log
```

Then:

```text
/error
```

## Watch application logs

```bash
tail -f app.log
```

## Create a configuration directory

```bash
mkdir -p app/config
```

## Create a backup

```bash
cp app.conf app.conf.backup
```

## Edit a configuration

```bash
vi app.conf
```

## Rename a configuration

```bash
mv app.conf app.conf.old
```

## Create a simple file

```bash
echo "server=nginx" > app.conf
```

Add another setting:

```bash
echo "port=8080" >> app.conf
```

---

# 36. Hands-On Activity 1 — Directory and File Management

Create the following structure:

```text
cloud-lab/
├── configs/
├── logs/
├── scripts/
└── backup/
```

Tasks:

1. Create the directory structure.
2. Create three configuration files inside `configs`.
3. Create two log files inside `logs`.
4. Create one shell script inside `scripts`.
5. Rename one configuration file.
6. Copy one configuration file into `backup`.
7. Move one log file into `backup`.
8. Verify everything using `ls`.
9. Use `pwd` to verify your location.
10. Create an empty directory and remove it with `rmdir`.
11. Create a directory containing a file and try `rmdir`.
12. Remove the non-empty directory using `rm -r`.

---

# 37. Hands-On Activity 2 — `vi` and File Editing

Create:

```text
cloud-lab/configs/app.conf
```

Add configuration such as:

```text
server=nginx
port=8080
environment=development
```

Practice:

1. Open the file with `vi`.
2. Enter Insert Mode using `i`.
3. Edit the configuration.
4. Use `Esc` to return to Normal Mode.
5. Search for `port`.
6. Move around using arrow keys.
7. Delete a complete line using `dd`.
8. Undo using `u`.
9. Copy a line using `yy`.
10. Paste using `p`.
11. Save using `:w`.
12. Exit using `:q`.
13. Open it again and practice `:q!`.
14. Practice `:%s/dev/prod/g`.

---

# 38. Hands-On Activity 3 — Server Log Practice

Create:

```text
server-lab/
├── app/
├── config/
├── scripts/
├── logs/
└── backup/
```

Tasks:

1. Create the directory structure.
2. Create an application configuration file.
3. Create a shell script file.
4. Create a log file containing at least 10 lines.
5. View the log using `cat`.
6. View the first few lines using `head`.
7. View the last few lines using `tail`.
8. Open the log using `less`.
9. Search inside the file.
10. Use `tail -f` and add new log entries from another terminal.
11. Copy the configuration file into `backup`.
12. Rename the original configuration file.
13. Edit the configuration using `vi`.
14. Search for a configuration setting.
15. Delete an unnecessary file.
16. Remove an empty directory using `rmdir`.
17. Remove a non-empty test directory using `rm -r`.

---

# 39. Final File Management Checklist

You should be comfortable with:

## Navigation

```text
ls
cd
pwd
```

## Directories

```text
mkdir
mkdir -p
rmdir
```

## Files

```text
rm
rm -r
cp
cp -r
mv
```

## Viewing

```text
cat
tac
less
more
head
tail
tail -f
```

## Editing

```text
nano
vi
```

## `vi` basics

```text
i
Esc
0
$
gg
G
/word
n
N
dd
yy
p
u
:w
:q
:wq
:q!
```

## Writing files

```text
echo
>
>>
```

---

# 40. What Matters Most for a Cloud Engineer

You don't need to memorize every Linux command.

The important goal is to be able to work comfortably from a terminal:

```text
Navigate
   ↓
Find files
   ↓
Read files
   ↓
Edit configuration
   ↓
Copy/backup files
   ↓
Move/rename files
   ↓
Inspect logs
   ↓
Clean up files
```

A typical real-world workflow might look like:

```bash
cd /etc/myapp
ls -la
cat app.conf
cp app.conf app.conf.backup
vi app.conf
tail -f /var/log/myapp.log
```

These are the kinds of file-management skills you will repeatedly use while working with Linux servers, cloud VMs, Docker containers, and DevOps environments.
