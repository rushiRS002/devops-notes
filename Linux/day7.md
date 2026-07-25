- https://openvim.com
- https://vim-adventures.com
## Mastering Text Editing with VIM 
## nano; piko; vi; vim 
## modes of vim edditor 

# Vim Editor Fundamentals

---
<img width="745" height="291" alt="Screenshot 2026-03-20 at 2 12 06 PM" src="https://github.com/user-attachments/assets/6fca116e-5580-4118-8975-8f593836fa08" />

## Overview of Vim & History

**Vim (Vi Improved)** is a powerful terminal-based text editor created by **Bram Moolenaar** in 1991.  
It is widely used by Linux system administrators and DevOps engineers.

---

## Vim Modes

| Mode | Purpose |
|----|--------|
| Command Mode | Default mode for navigation & commands |
| Insert Mode | Used to edit text |
| Visual Mode | Select text |
| Execute Mode | Run commands |

---

## Navigation in Command Mode

| Key | Action |
|----|-------|
| `h` | Left |
| `l` | Right |
| `k` | Up |
| `j` | Down |
| `gg` | Go to top |
| `G` | Go to bottom |
| `0` | Start of line |
| `$` | End of line |

---

## Text Manipulation

| Command | Action |
|-------|-------|
| `yy` | Copy line |
| `dd` | Delete line |
| `p` | Paste |
| `x` | Delete character |
| `dw` | Delete word |

---

## Undo & Redo

| Command | Action |
|------|------|
| `u` | Undo |
| `Ctrl + r` | Redo |

---

## Entering Insert Mode

| Key | Action |
|----|--------|
| `i` | Insert before cursor |
| `a` | Insert after cursor |
| `o` | New line below |
| `O` | New line above |

---

##  Editing in Insert Mode

- Type text normally
- Use arrow keys for movement
- Use Backspace/Delete to edit

---

##  Exiting Insert Mode

Press:

```bash
Esc
```

---

## Save & Exit

| Command | Purpose |
|-------|--------|
| `:w` | Save |
| `:q` | Quit |
| `:wq` | Save & quit |
| `:q!` | Force quit |

---
