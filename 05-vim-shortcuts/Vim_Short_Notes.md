# Vim — Short Notes

## 1. What is Vim?

**Vim (Vi Improved)** is a terminal-based text editor used in Linux.

Common uses:

- Configuration files
- Shell scripts
- YAML files
- Server/application configuration

Open a file:

```bash
vim filename
```

---

## 2. Vim Modes

### Normal Mode

Used for:

- Navigation
- Search
- Delete
- Copy/paste
- Undo
- Save/quit commands

You normally start in Normal Mode.

### Insert Mode

Used for typing and editing.

Enter:

```text
i
```

Return to Normal Mode:

```text
Esc
```

You can use **Backspace/Delete** normally while editing.

### Command-line Mode

Press:

```text
:
```

Used for save/quit and other Vim commands.

---

## 3. Basic Workflow

```text
vim file
   ↓
i
   ↓
Edit
   ↓
Esc
   ↓
:wq
```

---

## 4. Navigation

Arrow keys can be used for basic movement.

Traditional Vim navigation:

```text
h → left
j → down
k → up
l → right
```

Useful commands:

```text
0  → beginning of line
$  → end of line
gg → beginning of file
G  → end of file
```

`hjkl` is useful to know, but it does not need to be a priority initially.

---

## 5. Search

Search forward:

```text
/word
```

Next match:

```text
n
```

Previous match:

```text
N
```

Example:

```text
/port
```

---

## 6. Editing Commands

Delete current line:

```text
dd
```

Copy current line:

```text
yy
```

Paste:

```text
p
```

Undo:

```text
u
```

Redo:

```text
Ctrl+r
```

---

## 7. Save and Exit

Save:

```text
:w
```

Quit:

```text
:q
```

Save and quit:

```text
:wq
```

Quit without saving:

```text
:q!
```

---

## 8. Search and Replace

Replace all occurrences in the file:

```text
:%s/old/new/g
```

Example:

```text
:%s/dev/prod/g
```

---

## 9. What to Learn First for Cloud Engineering

### Must Know

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

### Learn Later

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

These are mostly convenience/advanced commands. You can navigate and use `i` for basic editing without memorizing all of them.

---

## 10. Quick Cheat Sheet

| Command | Purpose |
|---|---|
| `i` | Enter Insert Mode |
| `Esc` | Return to Normal Mode |
| `0` | Beginning of line |
| `$` | End of line |
| `gg` | Beginning of file |
| `G` | End of file |
| `/word` | Search |
| `n` | Next match |
| `N` | Previous match |
| `dd` | Delete line |
| `yy` | Copy line |
| `p` | Paste |
| `u` | Undo |
| `Ctrl+r` | Redo |
| `:w` | Save |
| `:q` | Quit |
| `:wq` | Save and quit |
| `:q!` | Quit without saving |
| `:%s/old/new/g` | Replace throughout file |
