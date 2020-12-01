usage
=====
- Category: tools/vim
- Tags: 
- Created: 2020-11-04T16:11:56+08:00

### New  and  Save
---

```bash
:new file   # new window above current window
:vert new file # new window right current window
:e filename # edit file
:w          # write current file
:w filename # write to another file
:q          # quit and close current file
:q!         # without and alert
:x          # write and quit
:wa         # write all window file
:qa         # quit all window
:xa         # write and quit all window
:qa!        # quit all window without alert
```

### Move and more
---

```bash
* gg moves to first line, G moves to last line, 123G moves to line number 123.
* Use u to undo and Ctrl-r to redo, multiple times.
```

### Run shell command
---

```bash
:! cmd      # execute an command
:! %        # execute current file
:!          # ececute last command
:r !cmd     # insert command output into current file
:silent !cmd    # ignore the enter hit when cmd done
```

### Window Operation
---

__Move__
  
>`CTRL+W` then `W` `H` `J` `K` `L` to move `Next` `Left` `Below` `Above` `Right` Window

__Split__

- `:vsp`            # without file
- `:vsp filename`   # vertical split window
- `:sp filename`    # horizontal split window