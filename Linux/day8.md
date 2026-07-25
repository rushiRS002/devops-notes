## The Editor's Lair: Mastering Text Editing with VIM
# Vim Editor – Execute & Visual Mode

---

## Entering Execute Mode

Execute mode is used to run commands that start with `:`  
To enter:
- esc ---> : 

---

## Basic Execute Mode Commands

### File Operations

| Command | Purpose |
|-------|--------|
| `:w` | Save file |
| `:q` | Quit |
| `:wq` | Save & quit |
| `:q!` | Force quit |
| `:e file` | Open file |
| `:r file` | Insert another file |

---

### Searching

| Command | Action |
|-------|------|
| `/word` | Search forward |
| `?word` | Search backward |
| `n` | Next result |
| `N` | Previous result |

---

### Line Numbering

| Command | Purpose |
|------|--------|
| `:set number` | Show line numbers |
| `:set nonumber` | Hide line numbers |
| `:10` | Jump to line 10 |

---

## Entering Visual Mode

| Key | Mode |
|----|-----|
| `v` | Character selection |
| `V` | Line selection |
| `Ctrl + v` | Block selection |

---

## Manipulating Text in Visual Mode

| Command | Action |
|-------|------|
| `y` | Copy |
| `d` | Delete |
| `p` | Paste |
| `>` | Indent right |
| `<` | Indent left |

---

## Vim Editor Revision Summary

| Mode | Use |
|----|----|
| Command | Navigation & control |
| Insert | Text editing |
| Visual | Text selection |
| Execute | Run commands |

---

## Must Remember Shortcuts

| Shortcut | Action |
|--------|-------|
| `Esc` | Return to command mode |
| `:wq` | Save & exit |
| `/` | Search |
| `dd` | Delete line |
| `yy` | Copy line |
| `p` | Paste |

---
