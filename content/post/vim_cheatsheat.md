+++
date = '2025-06-16T22:28:03+01:00'
tags = ["cheatsheet"]
categories = ["Cheatsheet", "Tutorial"]
description = 'A very informative Vim cheat sheet.'
series = ["Tech"]
title = 'Vim_Cheatsheet'
+++

# Comprehensive Vim / Neovim Cheatsheet

---

## 1. Modes
| Mode           | Description                      | How to Enter                  |
|----------------|---------------------------------|------------------------------|
| Normal         | Command mode (default)           | `Esc` or start Vim           |
| Insert         | Insert text                     | `i`, `a`, `I`, `A`, `o`, `O`|
| Visual         | Select text                    | `v` (char), `V` (line), `Ctrl+v` (block) |
| Command-line   | Enter commands (`:`)             | `:`                          |
| Replace        | Replace mode                    | `R`                          |
| Select         | Select mode (like Visual)       | `gh`                         |

---

## 2. Basic Movement
| Command       | Description                       |
|---------------|----------------------------------|
| `h`           | Left                             |
| `j`           | Down                             |
| `k`           | Up                               |
| `l`           | Right                            |
| `0`           | Beginning of line                |
| `^`           | First non-blank character       |
| `$`           | End of line                     |
| `w`           | Next word start                 |
| `e`           | End of word                    |
| `b`           | Previous word start             |
| `gg`          | Go to first line               |
| `G`           | Go to last line                |
| `Ctrl + d`    | Scroll down half page          |
| `Ctrl + u`    | Scroll up half page            |
| `Ctrl + f`    | Scroll down full page          |
| `Ctrl + b`    | Scroll up full page            |
| `%`           | Jump to matching bracket        |

---

## 3. Advanced Movement
| Command               | Description                   |
|-----------------------|------------------------------|
| `H`                   | Top of screen                |
| `M`                   | Middle of screen             |
| `L`                   | Bottom of screen             |
| `:n`                  | Go to line number `n`        |
| `fx`                  | Find next occurrence of `x`  |
| `Fx`                  | Find previous occurrence of `x` |
| `tx`                  | Move before next `x`          |
| `Tx`                  | Move before previous `x`      |
| `Ctrl + o`            | Jump to older position (jump back) |
| `Ctrl + i`            | Jump to newer position (jump forward) |

---

## 4. Editing Text
| Command           | Description                   |
|-------------------|------------------------------|
| `i`               | Insert before cursor          |
| `I`               | Insert at beginning of line  |
| `a`               | Append after cursor          |
| `A`               | Append at end of line        |
| `o`               | Open new line below and insert |
| `O`               | Open new line above and insert |
| `r<char>`         | Replace one character        |
| `R`               | Enter Replace mode           |
| `cw`              | Change word                  |
| `c$`              | Change to end of line        |
| `cc`              | Change (replace) entire line |
| `s`               | Substitute character and enter insert mode |
| `S`               | Substitute entire line and enter insert mode |

---

## 5. Deleting Text
| Command           | Description                   |
|-------------------|------------------------------|
| `x`               | Delete character under cursor |
| `X`               | Delete character before cursor |
| `dw`              | Delete word                  |
| `d$`              | Delete to end of line        |
| `dd`              | Delete entire line           |
| `d^`              | Delete to beginning of line  |
| `D`               | Delete to end of line        |
| `dG`              | Delete from current line to end of file |
| `dgg`             | Delete from current line to beginning of file |

---

## 6. Copying & Pasting
| Command           | Description                   |
|-------------------|------------------------------|
| `yy` or `Y`       | Yank (copy) current line      |
| `yw`              | Yank word                    |
| `y$`              | Yank to end of line          |
| `p`               | Paste after cursor           |
| `P`               | Paste before cursor          |
| `"+y`             | Yank to system clipboard     |
| `"+p`             | Paste from system clipboard  |

---

## 7. Visual Mode Commands
| Command           | Description                   |
|-------------------|------------------------------|
| `v`               | Enter visual (char) mode      |
| `V`               | Enter visual line mode        |
| `Ctrl + v`        | Enter visual block mode       |
| `y`               | Yank selected text            |
| `d`               | Delete selected text          |
| `c`               | Change selected text          |
| `>`               | Indent selection              |
| `<`               | Unindent selection            |
| `~`               | Toggle case of selection      |

---

## 8. Undo / Redo
| Command           | Description                   |
|-------------------|------------------------------|
| `u`               | Undo last change              |
| `U`               | Undo all changes on current line |
| `Ctrl + r`        | Redo undone change            |

---

## 9. Searching & Replacing
| Command           | Description                   |
|-------------------|------------------------------|
| `/pattern`        | Search forward for pattern    |
| `?pattern`        | Search backward for pattern   |
| `n`               | Repeat search forward         |
| `N`               | Repeat search backward        |
| `*`               | Search for word under cursor (forward) |
| `#`               | Search for word under cursor (backward) |
| `:%s/old/new/g`   | Replace all occurrences of 'old' with 'new' in file |
| `:%s/old/new/gc`  | Same with confirmation        |
| `:noh`            | Remove highlighting of search |

---

## 10. Registers
| Command           | Description                   |
|-------------------|------------------------------|
| `"ayy`            | Yank line into register `a`  |
| `"ap`             | Paste from register `a`       |
| `""`              | Default register             |
| `"_`              | Black hole register (discard) |

---

## 11. Macros
| Command           | Description                   |
|-------------------|------------------------------|
| `qa`              | Start recording macro in register `a` |
| `q`               | Stop recording                |
| `@a`              | Play macro in register `a`    |
| `@@`              | Repeat last macro            |

---

## 12. Buffers & Files
| Command           | Description                   |
|-------------------|------------------------------|
| `:e filename`     | Edit/open a file              |
| `:w`              | Save current file            |
| `:wa`             | Save all open files          |
| `:q`              | Quit current window          |
| `:q!`             | Quit without saving          |
| `:wq` or `:x`     | Save and quit                |
| `:bnext` or `:bn` | Go to next buffer            |
| `:bprev` or `:bp` | Go to previous buffer        |
| `:bd`             | Delete (close) buffer        |

---

## 13. Windows (Splits)
| Command           | Description                   |
|-------------------|------------------------------|
| `:split` or `:sp` | Horizontal split              |
| `:vsplit` or `:vsp`| Vertical split               |
| `Ctrl + w s`      | Horizontal split              |
| `Ctrl + w v`      | Vertical split                |
| `Ctrl + w w`      | Switch to next window         |
| `Ctrl + w h/j/k/l`| Move cursor between splits    |
| `Ctrl + w q`      | Close current split           |
| `Ctrl + w =`      | Make splits equal size        |

---

## 14. Tabs
| Command           | Description                   |
|-------------------|------------------------------|
| `:tabnew`         | Open new tab                 |
| `:tabclose`       | Close current tab            |
| `:tabnext` or `:tabn` | Go to next tab           |
| `:tabprev` or `:tabp` | Go to previous tab       |
| `gt`              | Go to next tab               |
| `gT`              | Go to previous tab           |

---

## 15. Marks & Jumps
| Command           | Description                   |
|-------------------|------------------------------|
| `ma`              | Set mark `a` at cursor        |
| `` `a``           | Jump to mark `a` (exact pos)  |
| `'a`              | Jump to mark `a` (start of line) |
| `` ` ``            | Jump back to last jump position |
| `Ctrl + o`        | Jump to older cursor position |
| `Ctrl + i`        | Jump to newer cursor position |

---

## 16. Ex Commands (colon commands)
| Command           | Description                   |
|-------------------|------------------------------|
| `:w`              | Save file                    |
| `:q`              | Quit                        |
| `:wq`             | Save and quit               |
| `:q!`             | Quit without saving         |
| `:e filename`     | Open file                   |
| `:ls` or `:buffers` | List buffers               |
| `:set option`     | Set an option               |
| `:set nooption`   | Disable an option           |
| `:syntax on`      | Enable syntax highlighting  |
| `:syntax off`     | Disable syntax highlighting |
| `:help keyword`   | Open help for keyword       |

---

## 17. Settings (common `:set` options)
| Option            | Description                   |
|-------------------|------------------------------|
| `:set number`     | Show line numbers             |
| `:set relativenumber` | Show relative line numbers  |
| `:set nowrap`     | Disable line wrapping         |
| `:set wrap`       | Enable line wrapping          |
| `:set ignorecase` | Case-insensitive search      |
| `:set smartcase`  | Case-sensitive if uppercase used |
| `:set cursorline` | Highlight current line       |
| `:set list`       | Show invisible chars (tabs, eol) |
| `:set tabstop=4`  | Set tab width                |
| `:set shiftwidth=4` | Indentation width          |
| `:set expandtab`  | Convert tabs to spaces       |
| `:set autoindent` | Auto-indent new lines        |
| `:set clipboard=unnamedplus` | Use system clipboard |

---

## 18. Miscellaneous
| Command           | Description                   |
|-------------------|------------------------------|
| `.`               | Repeat last command           |
| `Ctrl + g`        | Show file info                |
| `:!command`       | Run external shell command    |
| `:r filename`     | Read file content into current buffer |
| `:diffsplit file` | Open file in diff mode        |

---

*This cheatsheet covers a comprehensive range of Vim/Neovim commands and workflows to help you master the editor.*  

---
